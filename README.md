# action-nx-code-coverage

This is a github action for combining/compiling code coverage from an nx monorepo (although will probably work for other mono/single repos).

- This will loop through the coverage folder which should contain all the projects and their coverage results.
- On a PR execution, this will update the PR with a PR Commit displaying the coverage results for those projects
- On a push execution, this will update coverage badges for all projects (using the gist method specified [here](https://dev.to/thejaredwilcurt/coverage-badge-with-github-actions-finally-59fa)

> [GitHub Action](https://help.github.com/en/actions) to combine/compile code coverage from an nx monorepo

- ✅ &nbsp;Code coverage comment for monorepo
- ✅ &nbsp;Code coverage diff if base provided
- ✅ &nbsp;Updates the Code coverage comment instead of adding new

The report is based on the coverage report generated by your test runner.

## Inputs

```yaml
  github-token: 
    required: true
    description: Github token
  no-coverage-ran: 
    required: false
    description: true/false (default false) if no coverage was ran; useful if using nx affected and it returned no projects
    type: boolean
  coverage-folder: 
    required: false
    description: path to folder containing project folders with coverage data (default 'coverage')
  coverage-base-folder: 
    required: false
    description: path to folder containing project folders with coverage data for diff/base comparison (default 'coverage-base')
  gist-processing:
    required: false
    description: true/false (default true) should save results/badge to gist
    type: boolean
  gist-token:
    required: false
    description: 'Your secret with the gist scope for coverage badge; required if gist-processing not false'
  gist-id:
    required: false
    description: 'The ID of the gist to use; required if gist-processing not false'
  label-color:
    description: 'The left color of the badge'
    required: false
  color:
    description: 'The right color of the badge'
    required: false
  is-error:
    description: 'The color will be red and cannot be overridden'
    required: false
  named-logo:
    description: 'A logo name from simpleicons.org'
    required: false
  logo-svg:
    description: 'An svg-string to be used as logo'
    required: false
  logo-color:
    description: 'The color for the logo'
    required: false
  logo-width:
    description: 'The space allocated for the logo'
    required: false
  logo-position:
    description: 'The position of the logo'
    required: false
  style:
    description: 'The style like "flat" or "social"'
    required: false
  cache-seconds:
    description: 'The cache lifetime in seconds (must be greater than 300)'
    required: false
```

## Setup

- Perform gist setup from [link](https://dev.to/thejaredwilcurt/coverage-badge-with-github-actions-finally-59fa)
- Must run nx:affected test --coverage so the coverage results already exist
  
```yaml
      - name: Comment Code Coverage on PR
        uses: dkhunt27/nx-code-coverage@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          no-coverage-ran: false
          coverage-folder: ./coverage
          coverage-base-folder: ./coverage-base
          gist-processing: true
          gist-token: ${{ secrets.COVERAGE_BADGES_GIST_TOKEN }}
          gist-id: d7e6b443a01eba615fd2a7c1215aec91
          color: green
          named-logo: jest
```

- See sample workflows
  - [main-push.yaml](./.github/samples/main-push.yaml)
  - [pull-request.yaml](./.github/samples/pull-request.yaml)
  
## Making changes and pushing releases

- make new branch and make changes
- `npm run all`
- `git commit/push changes`
- make PR back to main
- wait for pipelines to finish (test will always finish with an error since this isn't a nx monorepo)
- `git checkout main`
- `git pull`
- `git tag v1`
- `SKIP_HOOKS=true git push origin v1`
- in github, edit tag and save (this will push to marketplace)

## NPM Check

```bash
npm run npm:check
```

## Acknowledgements

Thanks to the creators of the code that this was based on

- [eeshdarthvader/code-coverage-assistant](https://github.com/eeshdarthvader/code-coverage-assistant)
- [romeovs/lcov-reporter-action](https://github.com/romeovs/lcov-reporter-action).
- [ziishaned/jest-reporter-action](https://github.com/ziishaned/jest-reporter-action)
- [slavcodev/coverage-monitor-action](https://github.com/slavcodev/coverage-monitor-action)

## License

[MIT](LICENSE)
