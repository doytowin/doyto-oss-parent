DOYToWin Organization-Level POM
---

## Introduction
This project contains share parent POM for projects in DOYToWin repository.

## Usage
When starting a new project, use the already configured _parent pom_ to avoid replicate the same meta inf in every project:
```xml
<parent>
    <groupId>win.doyto</groupId>
    <artifactId>doyto-oss-parent</artifactId>
    <version>4</version>
    <relativePath/>
</parent>
```

Using [template](https://github.com/doytowin/doyto-query-template) to create new repository will automatically contain this.

## Release Process

1. Push local main branch to remote release branch.
2. Check the Release workflow, if
   1. success, release the package on https://oss.sonatype.org/.
   2. failed, edit local main branch and force push to remote release branch, repeat step 2.
3. Rebase remote release branch to local main branch.
4. Push local main branch to remote main branch.
