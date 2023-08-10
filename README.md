# DDEV add-on test init


A GitHub Action to initialize DDEV add-on tests.


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Quick start](#quick-start)
- [Inputs](#inputs)
  - [Available keys](#available-keys)
- [Usage](#usage)
  - [Test your DDEV add-on](#test-your-ddev-add-on)
- [License](#license)
- [Contribute](#contribute)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Quick start

_We will suppose here that you want to test your add-on with the stable version of DDEV._

You can add the following step in your workflow:

```yaml
- uses: julienloizelet/ddev-add-on-test-init@v0.2.0
  with:
    ddev_version: "stable"
```

This step will install the latest stable version of DDEV.

In the steps that follow, you will be able to run any DDEV commands.


## Inputs


### Available keys

The following keys are available as `step.with` keys:

---
- `ddev_version` (_String_)

DDEV version that will be installed before your tests.

Default: `stable`.

Allowed values are: `stable`, `HEAD`.

---

- `debug_enabled`

If `true`, a tmate session will be accessible before the tests step. See [action-tmate](https://github.com/mxschmitt/action-tmate) for more details.


Default: `false`.

---


- `token` (_String_)

A GitHub PAT required for the debug step.

A simple value could be: `${{ secrets.GITHUB_TOKEN }}`.

---




## Usage

### Test your DDEV add-on

If your add-on is based on the [DDEV add-on template repository](https://github.com/ddev/ddev-addon-template), you 
should have a tests folder containing a `test.bats` file.

Using this GitHub action, a  `.github/workflows/tests.yml` file could have the following content: 


```yaml
name: tests
on:
  pull_request:
  push:
    branches: [ main ]
    paths-ignore:
      - '**.md'

  schedule:
  - cron: '25 08 * * *'

  workflow_dispatch:
    inputs:
      debug_enabled:
        type: boolean
        description: Debug with tmate
        default: false

permissions:
  contents: write

defaults:
  run:
    shell: bash

jobs:
  tests:
    defaults:
      run:
        shell: bash

    strategy:
      matrix:
        ddev_version: [stable, HEAD]
      fail-fast: false

    runs-on: ubuntu-20.04

    steps:

    - uses: actions/checkout@v3

    - uses: julienloizelet/ddev-add-on-test-init@v0.2.0
      with:
        ddev_version: ${{ matrix.ddev_version }}
        token: ${{ secrets.GITHUB_TOKEN }}
        debug_enabled: ${{ github.event.inputs.debug_enabled }}

```


## License

[Apache](LICENSE)

## Contribute

Anyone is welcome to submit a pull request to this repository.


**Contributed and maintained by [julienloizelet](https://github.com/julienloizelet)**
