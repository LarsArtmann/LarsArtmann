# GitHub Profile Setup Guide

This document explains how to set up the dynamic metrics and statistics for this GitHub profile.

## Required Setup

### 1. Create a Personal Access Token (PAT)

The `lowlighter/metrics` action requires a GitHub Personal Access Token with appropriate permissions.

1. Go to [GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)](https://github.com/settings/tokens/new)
2. Create a new token with these scopes:
   - `repo` (Full control of private repositories)
   - `read:user` (Read all user profile data)
   - `read:org` (Read org and team membership, read org projects)
   - `user:email` (Access user email addresses)
3. Copy the generated token

### 2. Add the Token as a Repository Secret

1. Go to this repository's **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret**
3. Name: `METRICS_TOKEN`
4. Value: Paste your Personal Access Token
5. Click **Add secret**

### 3. Run the Workflow

After adding the secret:
1. Go to the **Actions** tab
2. Select the **Metrics** workflow
3. Click **Run workflow** → **Run workflow**

This will generate:
- `metrics.svg` - Main metrics dashboard
- `metrics.repositories.svg` - Featured repositories

## Features Enabled

| Feature | Plugin | Description |
|---------|--------|-------------|
| 📅 Isometric Calendar | `isocalendar` | Full-year commit activity visualization |
| 🈷️ Languages | `languages` | Top 8 languages with detailed analysis |
| 💡 Habits | `habits` | Coding habits and activity patterns |
| 🏆 Achievements | `achievements` | Compact achievement badges |
| 🎩 Notable | `notable` | Notable contributions |
| 📰 Activity | `activity` | Recent GitHub activity |
| 📓 Repositories | `repositories` | Featured project showcase |

## Dynamic Elements

The profile includes these external services:

| Service | Purpose |
|---------|---------|
| [github-profile-trophy](https://github.com/ryo-ma/github-profile-trophy) | Achievement trophies |
| [github-readme-stats](https://github.com/anuraghazra/github-readme-stats) | Stats cards & top languages |
| [github-readme-streak-stats](https://github.com/DenverCoder1/github-readme-streak-stats) | Contribution streak |
| [readme-typing-svg](https://github.com/DenverCoder1/readme-typing-svg) | Animated typing header |
| [capsule-render](https://github.com/kyechan99/capsule-render) | Header/footer waves |

## Future Enhancements (Issue #10)

For custom ObservableHQ graphs, consider:
- Forking [observablehq/oss-analytics](https://github.com/observablehq/oss-analytics/)
- Deploying to Observable Cloud
- Embedding custom visualizations

## Troubleshooting

### Metrics not updating?
- Verify `METRICS_TOKEN` secret is valid
- Check Actions tab for error logs
- Ensure token has correct permissions

### Images not loading?
- Run the workflow manually first
- Check if `metrics.svg` files exist in the repo
- Verify the image URLs in README.md match the generated files
