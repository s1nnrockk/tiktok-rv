# GitHub Actions setup

This fork uses GitHub Actions for repository checks, APK signing, release publishing, and optional VirusTotal uploads.

## Required signing secrets

Open **Settings → Secrets and variables → Actions → New repository secret** and add:

| Secret name | What it contains |
|---|---|
| `KEYSTORE_B64` | Base64-encoded contents of your long-term Android signing keystore |
| `KEYSTORE_PASSWORD` | Password for that keystore |
| `KEY_ALIAS` | Alias of the signing key inside the keystore |

Keep a secure offline backup of the keystore and its password. The same signing key should be reused for future updates; replacing it can prevent Android from installing an update over an existing installation.

The build now runs a preflight check before downloading dependencies. It stops early if any required signing secret is missing or if `KEYSTORE_B64` is not valid base64 data.

Never paste secret values into issues, pull requests, chat messages, workflow logs, or source files.

## Optional settings

- `VT_API_KEY` — VirusTotal API key. If it is absent, the release scan workflow reports a warning and skips the upload.
- `GPLAYDL_AUTH_JSON_B64` — optional base64-encoded gplaydl authentication JSON. The download job has a fallback when this is not configured.
- Repository variable `GPLAYDL_DISPENSER` — optional gplaydl dispenser setting.

The workflows use the built-in `GITHUB_TOKEN` for repository releases. A personal access token named `GH_PAT` is no longer required.

## Run a build

1. Open the repository's **Actions** tab.
2. Select **Automate TikTok Patching**.
3. Select **Run workflow**.
4. Choose the package variant and start the workflow.
5. Wait for the run to finish, then inspect every failed or skipped step before using a release asset.

The **Repository Quality Checks** workflow runs automatically on pushes and pull requests. It validates workflow YAML and checks the syntax of the local Bash and JavaScript helper scripts.

## Verify release files

Check the workflow run and release page before installing an APK. A VirusTotal report is only one signal: no scan result can guarantee that a modified application is safe. Review the source changes and build logs, and only use files from a repository and release you trust.
