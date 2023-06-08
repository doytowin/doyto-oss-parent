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
