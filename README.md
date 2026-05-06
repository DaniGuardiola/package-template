# My package template

This is my personal npm package template. It uses the latest and greatest tools while keeping things simple.

Here's what's included:

- [Bun](https://bun.sh/): fast package manager and task runner.
- [TypeScript](https://www.typescriptlang.org/): typed JavaScript.
- Plain `tsc`: ESM-only package compilation.
- [Changesets](https://github.com/changesets/changesets): versioning, changelogs and releases.
  - Includes a GitHub Action that automates changelogs and releases.
- [Oxlint](https://oxc.rs/docs/guide/usage/linter): type-aware linting.
- [Oxfmt](https://oxc.rs/docs/guide/usage/formatter): code formatting.
- Bug and feature request forms for GitHub issues.
- Automatic pull request CI checks: types, format, and linting.

## Usage

The easiest way to use this template is by using `bun create`:

```
bun create DaniGuardiola/package-template my-package
```

You can also use the "Use this template" button on GitHub.

## GitHub setup recipe

Most repository setup can be automated with the GitHub CLI. This recipe creates a repository from the template, configures the repository settings from the recommendations below, and applies branch protection to `main`.

Prerequisites:

- Install and authenticate [`gh`](https://cli.github.com/).
- Authenticate with a token that can create repositories and update repository administration settings.

```sh
OWNER="your-github-user-or-org"
REPO="my-package"
DESCRIPTION="My package description"

gh repo create "$OWNER/$REPO" \
  --template DaniGuardiola/package-template \
  --public \
  --clone \
  --description "$DESCRIPTION"

cd "$REPO"

gh repo edit "$OWNER/$REPO" \
  --allow-update-branch \
  --delete-branch-on-merge \
  --enable-auto-merge \
  --enable-merge-commit=false \
  --enable-rebase-merge=false \
  --enable-squash-merge \
  --squash-merge-commit-message pr-title

gh api \
  --method PUT \
  "repos/$OWNER/$REPO/branches/main/protection" \
  --input - <<'JSON'
{
  "required_status_checks": {
    "strict": true,
    "contexts": ["check-format", "check-types", "lint"]
  },
  "enforce_admins": false,
  "required_pull_request_reviews": {
    "dismiss_stale_reviews": false,
    "require_code_owner_reviews": false,
    "required_approving_review_count": 1,
    "require_last_push_approval": true
  },
  "restrictions": null,
  "required_conversation_resolution": false,
  "required_linear_history": false,
  "allow_force_pushes": false,
  "allow_deletions": false,
  "block_creations": false,
  "lock_branch": true,
  "allow_fork_syncing": true
}
JSON

gh api \
  --method PUT \
  "repos/$OWNER/$REPO/actions/permissions/workflow" \
  -F default_workflow_permissions=write \
  -F can_approve_pull_request_reviews=true
```

Then update package metadata, the license, changelog heading, and README content before publishing.

Once initialized, make sure to follow these steps:

- [ ] Update the name, description and author in the `package.json` file.
- [ ] Update the heading of the `CHANGELOG.md` file.
- [ ] Replace the author in the `LICENSE` file.
- [ ] Replace this README's content with your own.
- [ ] Publish to GitHub.
- [ ] Publish the initial version to npm from your local machine.

  npm trusted publishing requires the package to exist before it can be connected to a GitHub workflow.

  ```sh
  bun run build
  npm publish
  ```

  Complete the normal npm two-factor authentication prompt when asked.

- [ ] Authorize GitHub Actions to publish on npm.

  This is the npm-side permission grant. It lets this repository's `publish.yml` workflow publish with GitHub OIDC instead of a long-lived npm token.
  1. Go to the package page on npm.
  2. Open package settings.
  3. Open "Trusted publishers".
  4. Add a trusted publisher with provider "GitHub Actions".
  5. Set owner to your GitHub user or organization.
  6. Set repository to your repository name.
  7. Set workflow filename to `publish.yml`.
  8. Leave environment empty unless the workflow uses a GitHub environment.

- [ ] Enable the right permissions for the `GITHUB_TOKEN` secret:

  This is required for Changesets to create and update pull requests for versioning from the `publish.yml` workflow.
  1. In your GitHub repository, go to Settings > Actions > General.
  2. Scroll down to "Workflow permissions".
  3. Select the "Read and write permissions".
  4. Enable "Allow GitHub Actions to create and approve pull requests".

- [ ] Create a great package and publish it to npm! 🚀

## Releases

This template uses [Changesets](https://github.com/changesets/changesets) to manage releases. Check out their documentation to learn how to use it. The basic idea is:

1. Make changes.
2. Run `bun changeset` to create a new changeset.
3. Commit and push the changeset (either directly to `main` or by merging a pull request).
4. A PR titled "Version Packages" will be created (or updated) by the `publish.yml` workflow.
5. Merge the PR when you're ready to publish a new version.
6. The `publish.yml` workflow will publish the new version to npm.

## Recommendations

This is a list of settings and other things that I usually do in my packages. They are not mandatory though!

- General settings
  - [ ] Enable "Always suggest updating pull request branches".
  - [ ] Enable "Allow auto-merge".
  - [ ] Enable "Automatically delete head branches".
  - [ ] From "Allow merge commits/squash merging/rebase merging" leave only "Allow squash merging" enabled.
  - [ ] In the same setting, select "Default to pull request title".

- Main branch protection

  In your GitHub repository, go to Settings > Branches and click "Add rule".
  - [ ] In "Branch name pattern", type "main".
  - [ ] Enable "Require a pull request before merging".
  - [ ] Enable "Require approvals".
  - [ ] Enable "Require approval of the most recent reviewable push".
  - [ ] Enable "Require status checks to pass before merging".
  - [ ] Enable "Require branches to be up to date before merging".
  - [ ] Add the following status checks as required: `check-format`, `check-types`, `lint`.

  Finally, click "Create".

## Contributing

Contributions are welcome, but I will need to agree to significant changes since this is, after all, my personal template. Feel free to open an issue to discuss it.
