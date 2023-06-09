# doyto-oss-parent
DOYToWin organization-level POM 😏

## Introduction
This project contains share parent POM for projects in DOYToWin repository.

## Usage
When starting a new project, use the already configured _parent pom_ to avoid replicate the same meta inf in every project:
```xml
<parent>
    <groupId>win.doyto</groupId>
    <artifactId>doyto-oss-parent</artifactId>
    <version>3</version>
</parent>
```

Create `.github/workflows/release.yml` with following content to reuse the release workflow:

```yaml
name: Release

on:
  push:
    branches:
      - release

jobs:
  Call_Release_Workflow_in_doyto-oss-parent:
    uses: doytowin/doyto-oss-parent/.github/workflows/release.yml@main
    secrets: inherit
```

## Release Process

1. Push local main branch to remote release branch.
2. Check the Release workflow, if
   1. success, release the package on https://oss.sonatype.org/.
   2. failed, edit local main branch and force push to remote release branch, repeat step 2.
3. Rebase remote release branch to local main branch.
4. Push local main branch to remote main branch.
