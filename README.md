# github-tag-action

A Github Action to automatically bump and tag master, on merge, with the latest
SemVer formatted version.

[![Build Status](https://github.com/Viostream/github-tag-action/workflows/Bump%20version/badge.svg)](https://github.com/Viostream/github-tag-action/workflows/Bump%20version/badge.svg)
[![Stable Version](https://img.shields.io/github/v/tag/Viostream/github-tag-action)](https://img.shields.io/github/v/tag/Viostream/github-tag-action)
[![Latest Release](https://img.shields.io/github/v/release/Viostream/github-tag-action?color=%233D9970)](https://img.shields.io/github/v/release/Viostream/github-tag-action?color=%233D9970)

## Viostream customisations

This action is based on anothrNick/github-tag-action, but with Viostream
defaults applied to simplify our build flow.

### Usage

```Dockerfile
name: Bump version
on:
  push:
    branches:
      - master
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
      with:
        fetch-depth: '0'
    - name: Bump version and push tag
      uses: Viostream/github-tag-action@v2
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

_NOTE: set the fetch-depth for `actions/checkout@v2` to be sure you retrieve all
commits to look for the semver commit message._

### Options

#### Environment Variables

* **GITHUB_TOKEN** ***(required)*** - Required for permission to tag the repo.
* **DEFAULT_BUMP** *(optional)* - Which type of bump to use when none explicitly
  provided (default: `patch`).
* **WITH_V** *(optional)* - Set to false to not tag version with `v` character.
* **RELEASE_BRANCHES** *(optional)* - Comma separated list of branches (bash
  reg exp accepted) that will generate the release tags. Other branches and
  pull-requests generate versions postfixed with the commit hash and do not
  generate any tag. Examples: `master` or `.*` or `release.*,hotfix.*,master`.
* **CUSTOM_TAG** *(optional)* - Set a custom tag, useful when generating tag
  based on f.ex FROM image in a docker image. **Setting this tag will invalidate
  any other settings!**
* **SOURCE** *(optional)* - Operate on a relative path under $GITHUB_WORKSPACE.
* **DRY_RUN** *(optional)* - Determine the next version without tagging the
  branch. The workflow can use the outputs `new_tag` and `tag` in subsequent
  steps. Possible values are ```true``` and ```false``` (default).
* **INITIAL_VERSION** *(optional)* - Set initial version before bump. Default
  `0.0.0`.

#### Outputs

* **new_tag** - The value of the newly created tag.
* **tag** - The value of the latest tag after running this action.
* **part** - The part of version which was bumped.

### Bumping

**Manual Bumping:** Any commit message that includes `#major`, `#minor`, or
`#patch` will trigger the respective version bump. If two or more are present,
the highest-ranking one will take precedence.

**Automatic Bumping:** If no `#major`, `#minor` or `#patch` tag is contained in
the commit messages, it will bump whichever `DEFAULT_BUMP` is set to (which is
`patch` by default). Disable this by setting `DEFAULT_BUMP` to `none`.

> ***Note:*** This action **will not** bump the tag if the `HEAD` commit has
  already been tagged.

### Credits

* [fsaintjacques/semver-tool](https://github.com/fsaintjacques/semver-tool)
* [anothrNick/github-tag-action](https://github.com/anothrNick/github-tag-action)
