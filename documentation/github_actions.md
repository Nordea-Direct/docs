# Github actions

We use Github Actions to automate building, tagging and deploying our projects to Github Packages. This is a work in progress, but we have some proof-of-concepts running. 

## Table of Contents

* [Build stage](##build-stage)
* [Pre-Release stage](##pre-release-stage)
* [Release stage](##release-stage)

## Build stage

The build stage runs every time a push is performed on any branch (excluding tags). The purpose of this stage is to build the code, and check that the code builds correctly. This stage is split into multiple parts. 

First we define when and how the action will start

```yaml
on:
  push:
    branches:
      - '**'
    tags-ignore:
      - '*'
```

Then we set up the environment

```yaml
build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
```

After this, we define all the steps this workflow executes

### Setup JDK14

```yaml
    - uses: actions/checkout@v2
    - name: Set up JDK 14 for Maven Building
      uses: actions/setup-java@v2
      with:
        java-version: '14'
        distribution: 'adopt'
```

### Cache Maven Packages

This is performed so we dont have to download all the Maven packages

```yaml
    - name: Cache Maven packages
      uses: actions/cache@v2
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
        restore-keys: ${{ runner.os }}-m2
```

## Pre-Release stage

The Pre-Release stage

## Release stage

The Release stage
