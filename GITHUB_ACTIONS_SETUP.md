# GitHub Actions Setup Guide

Panduan lengkap untuk setup GitHub Actions workflow yang otomatis sync dan build design tokens dari Tokens Studio.

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Setup Steps](#setup-steps)
- [Workflow Details](#workflow-details)
- [Token Studio API Key](#tokens-studio-api-key)
- [Package.json Scripts](#packagejson-scripts)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

Sebelum setup, pastikan Anda memiliki:

- ✅ Tokens Studio account dan project setup
- ✅ Tokens Studio API Key (akan dibuat di langkah setup)
- ✅ GitHub repository dengan akses Settings
- ✅ Node.js 14+ installed locally
- ✅ npm atau yarn installed
- ✅ Style Dictionary config (sudah ada di `config.json`)

---

## Setup Steps

### Step 1: Create Tokens Studio API Key

1. Login ke [Tokens Studio](https://app.tokens.studio)
2. Go to **Settings** → **API Keys**
3. Click **Create New API Key**
4. Copy the API key (keep it safe!)
5. Note the organization and project details

### Step 2: Save API Key to GitHub Secrets

1. Go to GitHub repository
2. Click **Settings** tab (top menu bar)
3. Expand **Secrets and variables** in sidebar → **Actions**
4. Click **New repository secret**
5. Add the following:
   - **Name**: `TOKENS_STUDIO_API_KEY`
   - **Secret**: (paste your API key from Step 1)
6. Click **Add secret**

### Step 3: Setup Tokens Studio CLI Locally

Jika belum setup, jalankan di local machine:

```bash
# Install dependencies
npm install

# Setup Tokens Studio
npx tokensstudio setup --host graphql.prod.tokens.studio

# When prompted:
# - Enter your API Token (dari Step 1)
# - Select organization
# - Select project
# - This creates .tokensstudio.json

# Commit the config file
git add .tokensstudio.json
git commit -m "chore: Add Tokens Studio configuration"
git push
```

### Step 4: Setup Style Dictionary Build Script

Pastikan `package.json` memiliki build script:

```json
{
  "scripts": {
    "build": "style-dictionary build -c config.json",
    "build:watch": "style-dictionary build -c config.json --watch"
  }
}
```

### Step 5: Verify Workflow File

Workflow file sudah ada di `.github/workflows/sync-tokens.yml`

Verify dengan:
```bash
cat .github/workflows/sync-tokens.yml
```

### Step 6: Commit dan Push ke GitHub

```bash
git add .github/workflows/sync-tokens.yml
git commit -m "chore: Add GitHub Actions workflow for token sync and build"
git push origin main
```

---

## Workflow Details

### Trigger Events

Workflow otomatis dijalankan ketika:

1. **Push ke main branch** dengan changes di:
   - `tokens/**` - Token files
   - `config.json` - Style Dictionary config
   - `package.json` - Scripts
   - `.github/workflows/sync-tokens.yml` - Workflow file sendiri

2. **Manual trigger** via GitHub Actions UI:
   - Go ke **Actions** tab
   - Select **Sync & Build Design Tokens**
   - Click **Run workflow** → **Run workflow**

### Workflow Steps

```
1. 📥 Checkout repository
   ↓
2. ⚙️ Setup Node.js (v22)
   ↓
3. 📦 Install dependencies
   ↓
4. 🎨 Sync tokens from Tokens Studio
   (Pull latest tokens menggunakan TOKENS_STUDIO_API_KEY)
   ↓
5. ✅ Validate token structure
   (Check JSON validity dan file existence)
   ↓
6. 🔨 Build tokens for all platforms
   (Run: npm run build)
   Outputs:
   - CSS (variables)
   - SCSS (variables + maps)
   - JavaScript (CommonJS + ESM)
   - TypeScript (declarations)
   - JSON (raw)
   - Flutter (Dart)
   - Compose (Kotlin)
   - Android (XML)
   - iOS (Swift)
   ↓
7. 🔍 Check for changes
   (Detect if anything changed)
   ↓
8. 💾 Commit changes (if any)
   (Git commit dengan message)
   ↓
9. 📤 Push to main
   (Push directly ke main branch)
   ↓
10. 📊 Workflow Summary
    (Generate summary untuk GitHub Actions UI)
```

### Output Files

Setelah build berhasil, generated files ada di `build/` directory:

```
build/
├── web/
│   ├── css/
│   │   └── variables.css
│   ├── scss/
│   │   ├── variables.scss
│   │   └── maps.scss
│   ├── js/
│   │   ├── index.js (CommonJS)
│   │   └── index.esm.js (ESM)
│   └── ts/
│       └── index.d.ts
├── mobile/
│   ├── flutter/
│   │   └── tokens.dart
│   └── compose/
│       └── Tokens.kt
├── android/
│   └── values/
│       ├── colors.xml
│       └── dimens.xml
└── ios/
    └── Tokens.swift
```

---

## Tokens Studio API Key

### Security Best Practices

⚠️ **IMPORTANT:**

- ✅ **DO**: Store API key di GitHub Secrets
- ✅ **DO**: Rotate API key periodically
- ✅ **DO**: Restrict API key permissions di Tokens Studio
- ❌ **DON'T**: Commit `.env` files dengan API key
- ❌ **DON'T**: Share API key in messages/PR descriptions
- ❌ **DON'T**: Use personal API keys di shared projects

### Rotating API Key

Jika API key expired atau compromised:

1. Go to Tokens Studio Settings → API Keys
2. Delete old key
3. Create new key
4. Update GitHub secret:
   - Settings → Secrets → Edit `TOKENS_STUDIO_API_KEY`
   - Paste new key
5. Verify workflow runs successfully

---

## Package.json Scripts

Pastikan `package.json` memiliki scripts berikut:

```json
{
  "scripts": {
    "build": "style-dictionary build -c config.json",
    "build:watch": "style-dictionary build -c config.json --watch",
    "build:web": "style-dictionary build -c config.json --filter web",
    "build:mobile": "style-dictionary build -c config.json --filter mobile",
    "build:android": "style-dictionary build -c config.json --filter android",
    "build:ios": "style-dictionary build -c config.json --filter ios",
    "tokens:pull": "npx tokensstudio pull --host graphql.prod.tokens.studio",
    "tokens:sync": "npm run tokens:pull && npm run build"
  }
}
```

---

## Workflow Configuration

### Mengubah Trigger Events

Edit `.github/workflows/sync-tokens.yml` untuk mengubah kapan workflow dijalankan:

**Option 1: Hanya manual trigger**
```yaml
on:
  workflow_dispatch:
```

**Option 2: Push ke branch tertentu**
```yaml
on:
  push:
    branches:
      - main
      - develop
```

**Option 3: Scheduled (setiap hari pukul 8:00 UTC)**
```yaml
on:
  schedule:
    - cron: '0 8 * * *'
  workflow_dispatch:
```

**Option 4: Push + Pull Request**
```yaml
on:
  push:
    branches:
      - main
    paths:
      - "tokens/**"
  pull_request:
    paths:
      - "tokens/**"
  workflow_dispatch:
```

### Mengubah Branch Target

Edit step "Push changes to main" untuk push ke branch lain:

```yaml
- name: "📤 Push changes"
  run: git push origin develop  # ganti 'main' dengan branch name
```

---

## Monitoring & Logs

### View Workflow Runs

1. Go to GitHub repository
2. Click **Actions** tab
3. Select **Sync & Build Design Tokens**
4. Lihat list of workflow runs dengan:
   - ✅ Passed (hijau)
   - ❌ Failed (merah)
   - ⏳ In progress (kuning)

### View Detailed Logs

1. Click workflow run
2. Click **sync-and-build-tokens** job
3. Expand setiap step untuk lihat logs
4. Cari errors atau warnings

### Common Logs to Check

- **Validate token structure**: Pastikan JSON valid
- **Build tokens for all platforms**: Lihat build errors
- **Check for changes**: Pastikan detection bekerja
- **Commit changes**: Verify commit message
- **Push changes**: Verify push succeed

---

## Troubleshooting

### ❌ Workflow Fails: "TOKENS_STUDIO_API_KEY not found"

**Solution:**
1. Go to Settings → Secrets
2. Verify `TOKENS_STUDIO_API_KEY` exists
3. If not, create it per [Step 2](#step-2-save-api-key-to-github-secrets)
4. Rerun workflow

### ❌ Workflow Fails: "Invalid JSON"

**Solution:**
1. Check token files locally:
   ```bash
   node -e "JSON.parse(require('fs').readFileSync('tokens/modes/light.json', 'utf8'))"
   ```
2. Fix JSON errors
3. Commit dan push
4. Rerun workflow

### ❌ Workflow Fails: "style-dictionary build failed"

**Solution:**
1. Check `config.json` syntax
2. Verify all referenced token files exist
3. Run locally:
   ```bash
   npm run build
   ```
4. Fix errors locally first
5. Push dan rerun workflow

### ❌ Workflow Fails: "Git push rejected"

**Solution:**
1. Verify workflow has `contents: write` permission
2. Check `.github/workflows/sync-tokens.yml`:
   ```yaml
   permissions:
     contents: write
     pull-requests: write
   ```
3. Verify no branch protection rules blocking the push
4. Go to Settings → Branch protection → Check rules

### ⚠️ Workflow Succeeds But No Commit

**Likely cause:** No changes detected

**To verify:**
1. Check workflow logs
2. Look for: "No changes detected in tokens or build outputs"
3. This is normal if tokens didn't change

### ⚠️ API Key Expired

**Solution:**
1. Go to Tokens Studio Settings
2. Create new API key
3. Update GitHub secret
4. Rerun workflow

---

## Advanced: Customizing the Workflow

### Add Email Notifications

```yaml
- name: Send Notification
  if: always()
  uses: actions/github-script@v7
  with:
    script: |
      const status = context.payload.workflow_run.conclusion;
      console.log('Workflow ' + status);
```

### Add Slack Notifications

```yaml
- name: Notify Slack
  if: failure()
  uses: slackapi/slack-github-action@v1
  with:
    webhook-url: ${{ secrets.SLACK_WEBHOOK }}
    payload: |
      {
        "text": "Token sync failed!"
      }
```

### Add PR Comments

```yaml
- name: Comment on PR
  uses: actions/github-script@v7
  with:
    script: |
      github.rest.issues.createComment({
        issue_number: context.issue.number,
        owner: context.repo.owner,
        repo: context.repo.repo,
        body: '🎨 Tokens have been built and are ready!'
      })
```

---

## Resources

- [Tokens Studio CLI Documentation](https://documentation.tokens.studio/connect-studio-to-code/tokens-studio-cli)
- [Style Dictionary Documentation](https://styledictionary.com/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Secrets Documentation](https://docs.github.com/en/actions/security-guides/encrypted-secrets)

---

**Last Updated:** 2025-10-20
**Vuboi Design Token System**
