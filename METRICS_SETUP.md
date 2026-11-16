# GitHub profile metrics setup

The profile README references five SVG files committed to this repository. The files included in this package are lightweight placeholders; the workflow replaces them with generated analytics after it is configured and run successfully.

## 1. Create the metrics token

The `lowlighter/metrics` action requires a personal GitHub token because the repository-scoped `GITHUB_TOKEN` cannot read all account-level activity, contribution, and community data queried by Metrics.

1. Open **GitHub → Settings → Developer settings → Personal access tokens**.
2. Create a token compatible with the Metrics action's GraphQL/API requirements. Start with no additional scopes for public-account metrics.
3. Do not add private-repository access unless you explicitly want private data included. The profile is configured to show public-facing metrics only.

Metrics documents that individual plugins may require additional scopes. If a plugin reports a permission error, review that plugin's current documentation and add only the minimum scope it requires. Avoid granting the broad `repo` scope for this public profile setup.

## 2. Add the token as a repository secret

Open:

<https://github.com/tonysebastine/tonysebastine/settings/secrets/actions>

Create a **New repository secret** with:

- Name: `METRICS_TOKEN`
- Secret: paste the token value

Never put the token in `README.md`, a workflow file, a commit, or a chat message.

## 3. Run the workflow once

1. Open the repository's **Actions** tab.
2. Select **Refresh profile analytics**.
3. Select **Run workflow** on the `main` branch.
4. Wait for all five metric-generation steps to complete.
5. Confirm that these files have been updated by the workflow:
   - `metrics-overview.svg`
   - `metrics-community.svg`
   - `metrics-contributions.svg`
   - `metrics-languages.svg`
   - `metrics-habits.svg`

The workflow then refreshes the images daily at 00:17 UTC (05:47 India Standard Time). It can also be run manually at any time.

## What the profile measures

- Account, repository, community, and metadata summaries
- Recent public GitHub activity and follow-up of issues/pull requests
- Issue and pull-request follow-up ratios, plus GitHub achievements
- Isometric commit calendar with current streak and average commits per day
- Programming-language distribution across public repositories
- Recent GitHub activity patterns and coding-habit estimates

The habits panel is based on GitHub event activity. It does **not** measure editor/IDE time. For actual coding hours, add a WakaTime integration separately and store its API key as a repository secret.

## Security and maintenance

- The workflow grants only `contents: write`, which it needs to commit the generated SVG files.
- The Metrics action is pinned to the verified v3.34 commit SHA instead of a floating `@latest` or branch reference.
- The workflow runs on a schedule and manual dispatch only. It has no `push` trigger, avoiding self-trigger loops when the generated SVGs are committed.
- Plugin errors are non-fatal so a missing optional permission or unavailable plugin does not prevent the remaining panels from being generated.
- Review the upstream Metrics release notes before changing the pinned SHA or adding plugins.
