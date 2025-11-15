# action-towncrier

This repo contains a [reviewdog](https://github.com/reviewdog/reviewdog) action to run [towncrier](https://towncrier.readthedocs.io) to check for missing news fragments in a PR.

## Input

```yaml
inputs:
  github_token:
    description: 'GITHUB_TOKEN'
    default: '${{ github.token }}'
  workdir:
    description: 'Working directory relative to the root directory.'
    default: '.'
  ### Flags for reviewdog ###
  level:
    description: 'Report level for reviewdog [info,warning,error]'
    default: 'error'
  reporter:
    description: 'Reporter of reviewdog command [github-pr-check,github-check,github-pr-review].'
    default: 'github-pr-check'
  filter_mode:
    description: |
      Filtering mode for the reviewdog command [added,diff_context,file,nofilter].
      Default is added.
    default: 'added'
  fail_level:
    description: |
      If set to `none`, always use exit code 0 for reviewdog. Otherwise, exit code 1 for reviewdog if it finds at least 1 issue with severity greater than or equal to the given level.
      Possible values: [none,any,info,warning,error]
      Default is `none`.
    default: 'none'
  reviewdog_flags:
    description: 'Additional reviewdog flags'
    default: ''
  ### Flags for towncrier ###
  compare_with:
    description: "--compare-with flag of towncrier check command"
    default: "origin/main"
```

## Usage

```yaml
name: reviewdog
on: [pull_request]
jobs:
  linter_name:
    name: runner / towncrier
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
        with:
          fetch-depth: 0
      - uses: reviewdog/action-towncrier@v1.0.0 # v1.0.0
        with:
          github_token: ${{ secrets.github_token }}
          # Change reviewdog reporter if you need [github-pr-check,github-check,github-pr-review].
          reporter: github-pr-review
          # Change reporter level if you need.
          # GitHub Status Check won't become failure with warning.
          level: warning
          fail_level: any
          # Change the branch that towncrier uses for comparison
          compare_with: origin/main
```

> NOTE: Check out with fetch depth 0 required for towncrier

> NOTE: When towncrier is not installed already, this action will install the latest version from pip
