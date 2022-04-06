# combine-prs

A reusable GitHub Action to combine Pull Requests.

## Parameters

`branchPrefix` - The prefix of the branches which must be combined. Defaults to "dependabot".

`mustBeGreen` - "Only combine the pull requests that are green (status is success)". Defaults to "true".

`combineBranchName` - Name of the branch where combined pull requests will be merged. Defaults to "combine-prs-branch".

`ignoreLabel` - Ignore the pull requests with this label. Defaults to "nocombine".

`combinePullRequestTitle` - Title of the combined pull request. Defaults to "Combined Pull Request".

`closeCombinedPrs` - Close the pull requests that are successfully combined. Defaults to "true".

`githubToken` - Github token.

## Usage

Create a yml file in .github/workflow folder. For example combine-prs.yml. Add the configuration below and change the values as needed.

```yaml
name: "Combine Dependabot Pull Requests"

on:
  schedule:
    - cron: '00 10 * * 1' # Runs at 10 AM every Monday

  workflow_dispatch:

jobs:
  combine-prs:
    name: "Combine Dependabot Pull Requests"
    runs-on: ubuntu-20.04
    steps:
      - uses: hmcts/am-github-actions/combine-prs@master
        name: "Combine Dependabot Pull Requests"
        with:
          branchPrefix: "dependabot"
          mustBeGreen: "true"
          combineBranchName: "combine-prs-branch"
          ignoreLabel: "nocombine"
          combinePullRequestTitle: "Dependabot Combined Pull Request"
          closeCombinedPrs: "true"
          githubToken: "${{ secrets.GITHUB_TOKEN }}"

```

With the above configuration, the action automatically runs at 10 AM every Monday. Alternatively, this action can be executed manually from the Action tab of the repository.

## Note

- If a pull request merge fails due to conflicts, a label `automergeconflict` is added to the pull request. Developers can use this label to identify the failed pull request and resolve the merge issues.

- Often failed pull requests are successfully merged when, this action is executed again, after the combined pull request is merged with master branch.

## Example implementation

Refer Pull Request https://github.com/hmcts/am-org-role-mapping-service/pull/779