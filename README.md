# Awesome Scala Library Template

[![Scala Steward Action badge](https://img.shields.io/badge/Scala_Steward_Action-helping-blue.svg?style=flat&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAQCAMAAAARSr4IAAAAVFBMVEUAAACHjojlOy5NWlrKzcYRKjGFjIbp293YycuLa3pYY2LSqql4f3pCUFTgSjNodYRmcXUsPD/NTTbjRS+2jomhgnzNc223cGvZS0HaSD0XLjbaSjElhIr+AAAAAXRSTlMAQObYZgAAAHlJREFUCNdNyosOwyAIhWHAQS1Vt7a77/3fcxxdmv0xwmckutAR1nkm4ggbyEcg/wWmlGLDAA3oL50xi6fk5ffZ3E2E3QfZDCcCN2YtbEWZt+Drc6u6rlqv7Uk0LdKqqr5rk2UCRXOk0vmQKGfc94nOJyQjouF9H/wCc9gECEYfONoAAAAASUVORK5CYII=)](https://github.com/scala-steward-org/scala-steward-action)
[![Build status](https://github.com/bpg/typelevel-library.g8/workflows/build/badge.svg?branch=main)](https://github.com/bpg/typelevel-library.g8/actions?query=workflow%3Abuild)
[![Mergify Status](https://img.shields.io/endpoint.svg?url=https://gh.mergify.io/badges/bpg/awesome-scala-library.g8&style=flat)](https://mergify.io)

This is a [Giter8][g8] template for creating libraries ready to be published.

**NOTE:**
This project was started as a fork of <https://github.com/alexandru/typelevel-library.g8>, with some minor updates and improvements.
But at this point I don't think I will be merging it back to the upstream, as I have quite a few specific changes.

- [Usage](#usage)
  - [Setting Up GitHub (with `main` instead of `master`)](#setting-up-github-with-main-instead-of-master)
  - [Configuration of Automatic Releases to Sonatype](#configuration-of-automatic-releases-to-sonatype)
- [Sample Project](#sample-project)
- [Features](#features)
- [Template license](#template-license)

## Usage

Using [sbt](https://www.scala-sbt.org/download.html) run the following in a terminal:

```sh
sbt new bpg/awesome-scala-library.g8
```

### Setting Up GitHub

First initialize the local git repository, and do your first commit:

```sh
cd $project-folder/

git init

git commit -am 'Initial commit'
```

Create a new repository on GitHub, see [github.com/new](https://github.com/new) and add it as your origin:

```scala
git remote add origin https://github.com/$GITHUB_USERNAME/$GITHUB_REPOSITORY

git push -u origin main
```

### Configuration of Automatic Releases to Sonatype

The created project already has workflows defined for building and releasing the library on Sonatype via [GitHub Actions](https://github.com/features/actions). For automated releases to work, you need to configure:

- `GH_TOKEN` — for automatically publishing the documentation microsite with the `repo` scope:
  - see [GitHub's documentation](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line)
  - [Quick link (click here)](https://github.com/settings/tokens/new?scopes=repo&description=sbt-microsites)
- For publishing to Sonatype:
  - `GPG_PASSPHRASE`
  - `GPG_PRIVATE_KEY`
  - `SONATYPE_USERNAME`
  - `SONATYPE_PASSWORD`
  - See documentation at [sbt-ci-release](https://github.com/olafurpg/sbt-ci-release) for generating these

## Keep dependencies up to date

This template project, as well as created project, has a pre-configured [Scala Steward GitHub Action](https://github.com/scala-steward-org/scala-steward-action) that together with [Mergify Bot](https://mergify.io) keep dependencies up-to-date. For the action to work you also need secrets:

- `GH_TOKEN`
- `GPG_PASSPHRASE`
- `GPG_PRIVATE_KEY`

## Release drafting

The created project has a pre-configured [Release Drafter GitHub Action](https://github.com/release-drafter/release-drafter) to draft a nicely looking release notes based on PR labels.

It uses some custom labels to classify PRs, see `.github/release-drafter.yml` for more details. The labels can be created with help of [hub](https://github.com/github/hub) CLI using [the script](create-labels.sh):

```sh
#!/bin/sh
REPO="bpg/my-awesome-library"

hub api /repos/${REPO}/labels -X POST -f 'name=dependency' -f 'description=Dependency update' -f 'color=a8f49c'
hub api /repos/${REPO}/labels -X POST -f 'name=skip-changelog' -f 'description=Do not include this PR in the changelog' -f 'color=ffffdd'
hub api /repos/${REPO}/labels -X POST -f 'name=breaking' -f 'description=Backward-incompatible change, may break existing API clients' -f 'color=e2b236'
hub api /repos/${REPO}/labels -X POST -f 'name=maintenance' -f 'description=Maintenance / technical debt' -f 'color=371596'
hub api /repos/${REPO}/labels -X POST -f 'name=major' -f 'color=666666'
hub api /repos/${REPO}/labels -X POST -f 'name=minor' -f 'color=666666'
hub api /repos/${REPO}/labels -X POST -f 'name=patch' -f 'color=666666'
```

## Publish website

To publish the website to [GitHub Pages](https://pages.github.com/), it is recommended that you first create the `gh-pages` branch:

```sh
git checkout --orphan gh-pages
git rm --cached -r .
touch index.html && git add index.html
git commit -am 'Initial commit'
git push --set-upstream origin gh-pages
git add .
git reset --hard HEAD
git checkout main
```

## Sample Project

See sample library generated out of the box:

- [github repository](https://github.com/bpg/my-awesome-library)
- [documentation website](https://bpg.github.io/my-awesome-library/)
- [maven central artifacts](https://search.maven.org/artifact/com.github.bpg/my-awesome-library-core_2.13) (published via CI)

## Features

- Build setup for multiple sub-projects
- Sane Scala compiler defaults for doing FP (including [kind-projector](https://github.com/typelevel/kind-projector) and [better-monadic-for](https://github.com/oleg-py/better-monadic-for))
- Continuous integration via [GitHub Actions](https://github.com/features/actions)
  - With automated publishing to Sonatype!
- Usual [contributing](./src/main/g8/CONTRIBUTING.md), [code of conduct](./src/main/g8/CODE_OF_CONDUCT.md), [license](./src/main/g8/LICENSE.md) boilerplate
- [Scala.js](https://www.scala-js.org/) cross-compilation
- [sbt-crossproject](https://github.com/portable-scala/sbt-crossproject) for managing the JVM / JS configuration
- [sbt-unidoc](https://github.com/sbt/sbt-unidoc) for unifying the API documentation of the sub-projects
- [sbt-doctest](https://github.com/tkawachi/sbt-doctest) for testing the ScalaDoc
- [sbt-microsites](https://github.com/47deg/sbt-microsites) for building the documentation website, type checked via [mdoc](https://github.com/scalameta/mdoc)
- [sbt-header](https://github.com/sbt/sbt-header) for automatic copyright headers
- [sbt-scalafmt](https://github.com/scalameta/scalafmt) default setup for auto-formatting
- [sbt-tpolecat](https://github.com/DavidGregory084/sbt-tpolecat) for sane Scalac compiler options with most linter options on
- [sbt-ci-release](https://github.com/olafurpg/sbt-ci-release) for managing the versioning and the release to Sonatype
  - [sbt-git](https://github.com/sbt/sbt-git) setup for version management based on Git tags and SHAs
  - [sbt-dynver](https://github.com/dwijnand/sbt-dynver) a ready-made setup of `sbt-git` options for dynamic version management
  - [sbt-sonatype](https://github.com/xerial/sbt-sonatype) for faster and easier releases
- [sbt-scoverage](https://github.com/scoverage/sbt-scoverage) for code coverage with sane setup
- [Ammonite REPL](https://ammonite.io/) for quickly trying new stuff, run via `test:run`
- ...

Template license

Cloned from [scala/scala-seed][source], inspired by the build definition of [Monix][monix] and by [ChristopherDavenport/library.g8][library.g8], another template with similar goals.

To the extent possible under law, the author(s) have dedicated all copyright and related and neighboring rights to this template to the public domain worldwide. This template is distributed without any warranty. See <http://creativecommons.org/publicdomain/zero/1.0/>.

[g8]: http://www.foundweekends.org/giter8/
[monix]: https://monix.io
[source]: https://github.com/scala/scala-seed.g8
[library.g8]: https://github.com/ChristopherDavenport/library.g8/
