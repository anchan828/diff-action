# GitHub Actions: `diff-action` ![build-test](https://github.com/anchan828/diff-action/workflows/build-test/badge.svg)

This GitHub Action allows you to compare two files based on a tolerance, output the result to a file and send various notifications (comment on a linked GitHub issue/pull request, create a [Check Run](https://developer.github.com/v3/checks/runs/#create-a-check-run), ...), etc.

## Usage

The following shows how to compare two files, making sure only additions happened, and output the difference to a file.

```
name: "build-test"
on: [push]

jobs:
  test_something:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - uses: anchan828/diff-action@v0.1.5
      with:
        old: file1.txt
        new: file2.txt
        mode: addition
        tolerance: better
        output: out.txt
```

See the [examples workflow](.github/workflows/examples.yml) for more details and examples (and see the [associated runs](https://github.com/anchan828/diff-action/actions?query=workflow%3Aexamples) to see how it will look like).

## Inputs

### `old`

**Required** The first file to compare

### `new`

**Required** The second file to compare

### `mode`

**Required** The method used to measure the `tolerance`, can be either `addition` (addition are better) or `deletion` (deletion are better)

### `tolerance`

**Required** The tolerance to check the diff for, depends on `mode`, can be either (examples given with mode = `deletion`) `better` (only deletion), `mixed-better` (more deletion than addition), `same` (stay the exact same), `mixed` (same amount of lines but not the same), `mixed-worse` (more addition than deletion) or `worse` (only addition)

### `output`

_Optional_ The path where to output the diff (as well as on the console)

### `token`

_Optional_ Your `GITHUB_TOKEN`, **required** when using `notify_check` and/or `notify_issue`

### `title`

_Optional_ add a title to the notifications to distinguish between multiple workflows/jobs

### `notify_check`

_Optional_ Will create a [GitHub Check Run](https://developer.github.com/v3/checks/runs/#create-a-check-run) if `'true'` is specified, **requires** `token` to be given as well

### `notify_issue`

_Optional_ Will create a comment in the linked issue if `'true'` is specified, **requires** `token` to be given as well

## Outputs

### `passed`

Contains a boolean (`'true'` or `'false'`) representing if the check passed or not

### `output`

Contains the output of the diff

### `changed`

Contains a boolean value (`'true'` or `'false'`) representing if diff is present.

## Issues

 - Add a mode for the tolerance so `addition` can be better than `deletion` (which isn't possible now)
