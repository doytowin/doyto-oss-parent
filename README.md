# doyto-oss-parent
DOYToWin organization-level POM 😏

## Introduction
This project contains share parent POM for projects in DOYToWin repository.

## Usage
When starting a new project, please use the already configured _parent pom_ to avoid replicate the same meta inf in every project:
```xml
<parent>
    <groupId>win.doyto</groupId>
    <artifactId>doyto-oss-parent</artifactId>
    <version>3</version>
</parent>
```

### Release Process

1. Push local main branch to remote release branch.
2. Check the Release workflow, if
   1. success, release the package on https://oss.sonatype.org/.
   2. failed, edit local main branch and force push to remote release branch, repeat step 2.
3. Rebase remote release branch to local main branch.
4. Push local main branch to remote main branch.

### GitHub Release Workflow Template

```yaml
name: Release

on:
  push:
    branches:
      - release

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Configure Git User
        run: |
          git config user.email ${{ vars.GH_EMAIL }}
          git config user.name ${{ vars.GH_USERNAME }}
          git config user.signingkey ${{ vars.GPG_KEY_ID }}
          git config gpg.program gpg 
          git config commit.gpgsign true

      - name: Cache Maven packages
        uses: actions/cache@v3
        with:
          path: ~/.m2
          key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
          restore-keys: ${{ runner.os }}-m2

      - name: Set up JDK
        uses: actions/setup-java@v3
        with:
          java-version: '8'
          distribution: 'adopt'
          server-id: sonatype-nexus-staging # Value of the distributionManagement/repository/id field of the pom.xml
          server-username: OSSRH_USERNAME # env variable for username in deploy
          server-password: OSSRH_PASSWORD # env variable for token in deploy
          gpg-private-key: ${{ secrets.GPG_PRIVATE_KEY }} # Value of the GPG private key to import
          gpg-passphrase: GPG_PASSPHRASE # env variable for GPG private key passphrase

      - name: Deploy to Nexus
        env:
          GPG_TTY: $(tty)
          GPG_PASSPHRASE: ${{ secrets.GPG_PASSPHRASE }}
          OSSRH_USERNAME: ${{ secrets.OSSRH_USERNAME }}
          OSSRH_PASSWORD: ${{ secrets.OSSRH_PASSWORD }}
          GITHUB_ACTOR: ${{ github.actor }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          mvn -B clean \
          org.projectlombok:lombok-maven-plugin:1.18.20.0:delombok \
          -Dlombok.sourceDirectory=src/main/java \
          -Dlombok.outputDirectory=target/delombok \
          -Dlombok.addOutputDirectory=false \
          -DsourceDirectory=target/delombok \
          -DskipTests=true \
          release:prepare release:perform

      - name: Rollback on failure
        if: failure()
        env:
          GITHUB_ACTOR: ${{ github.actor }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: mvn -B release:rollback
```
