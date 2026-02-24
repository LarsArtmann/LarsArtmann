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
- `metrics.svg` - Main metrics dashboard (includes achievements)
- `metrics.repositories.svg` - Featured repositories

## Features Enabled

| Feature | Plugin | Description |
|---------|--------|-------------|
| 📅 Isometric Calendar | `isocalendar` | Full-year commit activity visualization |
| 🈷️ Languages | `languages` | Top 8 languages with detailed analysis |
| 🏆 Achievements | `achievements` | Detailed achievement badges |
| 🎩 Notable | `notable` | Notable contributions |
| 📓 Repositories | `repositories` | Featured project showcase |

### Private Repository Stats

With the `repo` scope on your PAT, the following will include private repository data:
- Language statistics
- Achievement badges
- Notable contributions
- Commit activity

## Dynamic Elements

The profile includes these external services:

| Service | Purpose |
|---------|---------|
| [github-profile-trophy](https://github.com/ryo-ma/github-profile-trophy) | Achievement trophies |
| [github-readme-streak-stats](https://github.com/DenverCoder1/github-readme-streak-stats) | Contribution streak |
| [readme-typing-svg](https://github.com/DenverCoder1/readme-typing-svg) | Animated typing header |
| [capsule-render](https://github.com/kyechan99/capsule-render) | Header/footer waves |

## Self-Hosting (Issue #13)

To include private repository stats in external services and avoid rate limits/outages, you can self-host:

### Vercel Deployment

1. **Trophies** - Fork [github-profile-trophy](https://github.com/ryo-ma/github-profile-trophy) and deploy to Vercel:
   - Add `GITHUB_TOKEN1` environment variable with your PAT (requires `repo` scope)
   - Update README URL to your Vercel deployment

2. **Streak Stats** - Fork [github-readme-streak-stats](https://github.com/DenverCoder1/github-readme-streak-stats) and deploy:
   - Configure with your PAT for private repo access
   - Update README URL accordingly

### Benefits of Self-Hosting

- No rate limits (personal API tokens)
- No outages from shared infrastructure
- Private repository statistics included
- Full control over caching and performance

## Troubleshooting

### Metrics not updating?
- Verify `METRICS_TOKEN` secret is valid and not expired
- Check Actions tab for error logs
- Ensure token has `repo` scope for private repo access

### Images not loading?
- Run the workflow manually first
- Check if `metrics.svg` files exist in the repo
- Verify the image URLs in README.md match the generated files

### External services returning 503?
- Consider self-hosting (see above)
- Check if community mirrors are available
- Verify GitHub API status
