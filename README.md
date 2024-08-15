# node-version-bumper
Github action for bump a new package version


# Usage
```yml
name: Release

on:
  workflow_dispatch:
    inputs:
      version:
        description: Release type
        required: false
        type: choice
        default: patch
        options:
          - major
          - minor
          - patch
          - premajor
          - preminor
          - prepatch
          - prerelease
      preid:
        description: Pre-id
        required: false
        type: choice
        options:
          - ''
          - dev
          - alpha
          - beta
          - rc

jobs:
  version:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Bump version
        id: bump
        uses: CheeseGrinder/node-version-bumper@v1
        with:
          version: ${{ inputs.version }}
          preid: ${{ inputs.preid }}
          commiter-name: Cheese Grinder CI

      - name: Commit & Tag
        run: |
          git push
          git tag ${{ steps.bump.outputs.version }}
          git push origin tag ${{ steps.bump.outputs.version }}
```

# Options
| name                | value                                                                                     | required           | default                         |
| ------------------- | ----------------------------------------------------------------------------------------- | ------------------ | ------------------------------- |
| `node-version`      | `"string"`                                                                                | :x:                | `"20.x"`                        |
| `npm-registry`      | `"string"`                                                                                | :x:                | `"https://registry.npmjs.org/"` |
| `working-directory` | `"string"`                                                                                | :x:                | `"."`                           |
| `is-workspace`      | `"boolean"`                                                                               | :x:                | `false`                         |
| `version`           | `"major" \| "minor" \| "patch" \| "premajor" \| "preminor" \| "preminor" \| "prerelease"` | :white_check_mark: |                                 |
| `preid`             | `"string"`                                                                                | :x:                | `""`                            |
| `commiter-name`     | `"string"`                                                                                | :x:                | `"Node Version Bumper"`         |
| `commiter-email`    | `"string"`                                                                                | :x:                | `"<>"`                          |
| `commit-message`    | `"string"`                                                                                | :x:                | `"ci(version): bump to v%s"`    |