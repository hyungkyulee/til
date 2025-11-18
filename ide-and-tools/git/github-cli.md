# Github cli to use credential

## installation
brew install gh

gh secret set --help

## manually run the workflow including some input values
> context : If you do not see the "Run workflow" button, it is likely because:
> The workflow file is not present in the default branch (usually main or master).
> The workflow file is only in your feature or PR branch.
> GitHub Actions only shows the "Run workflow" button for workflow files in the default branch.
> You can still trigger the workflow on your branch, but you must use the GitHub API or CLI.

### Workarounds:

1. Merge the workflow file into the default branch (even temporarily) to enable the UI button.
2. Use GitHub CLI:
   ```
   gh workflow run test-workflow.yml \
  --ref <your-branch-name> \
  --field environment=<environment>
  --field <key of inputs>=<value of inputs> \
  -- <other fields if required>
   ```
3. Use GitHub REST API: https://docs.github.com/en/rest/actions/workflows?apiVersion=2022-11-28#dispatch-a-workflow 
