# GitHub Actions Artifact → Telegram

A simple GitHub Actions workflow for downloading an artifact from a GitHub Actions run and sending it to a Telegram chat.

Because Telegram's standard Bot API currently limits `sendDocument` uploads to 50 MB, the workflow extracts the artifact ZIP and creates a multi-volume 7z archive with approximately 45 MB per volume. Each volume is then sent to Telegram separately. citeturn0search0turn0search4

## 🤖 Telegram Bot

Use **[@github_download_actions_bot](https://t.me/github_download_actions_bot)**.

The bot is used to obtain your Telegram `chat_id`.

Send the bot any message and use the returned Chat ID when running the GitHub Actions workflow.

## ⚙️ What the Action does

The workflow is manually triggered with three inputs:

1. **Run URL** — the URL of the GitHub Actions run containing the artifact.
2. **Artifact name** — the exact name of the artifact to download.
3. **Chat ID** — the Telegram chat ID where the artifact should be sent.

GitHub Actions supports manually triggered workflows with `workflow_dispatch` and custom inputs. citeturn0search1turn0search3

The workflow then:

```text
GitHub Actions Run URL
        ↓
Find artifact by name
        ↓
Download artifact.zip
        ↓
Extract artifact ZIP
        ↓
Create 45 MB 7z volumes
        ↓
artifact.7z.001
artifact.7z.002
artifact.7z.003
...
        ↓
Send every part to Telegram
```

## 🚀 Setup

### 1. Add the workflow

Place the workflow file in your repository:

```text
.github/workflows/send-artifact-to-telegram.yml
```

Use the workflow supplied with this project.

### 2. Create the Telegram bot secret

Go to:

**Repository → Settings → Secrets and variables → Actions**

Create a repository secret named:

```text
TELEGRAM_BOT_TOKEN
```

Set its value to your Telegram bot token.

**Never commit the bot token to the repository.**

### 3. Get your Chat ID

Open:

**[@github_download_actions_bot](https://t.me/github_download_actions_bot)**

Send it a message.

The bot will return your Chat ID, for example:

```text
🆔 Chat ID:

1984869330
```

Copy that number.

## ▶️ How to use

Go to:

**GitHub → Actions → Send Artifact to Telegram → Run workflow**

Enter:

### Run URL

Example:

```text
https://github.com/Wadamzmail/AndroidIDE/actions/runs/35450579170
```

### Artifact name

Enter the exact artifact name.

Example:

```text
apk-arm64-v8a-release
```

### Chat ID

Enter the Chat ID returned by the Telegram bot.

Example:

```text
1984869330
```

Then click **Run workflow**.

## 📦 What you receive

If the artifact is larger than Telegram's upload limit, it is split into multiple 7z volumes:

```text
apk-arm64-v8a-release.7z.001
apk-arm64-v8a-release.7z.002
apk-arm64-v8a-release.7z.003
apk-arm64-v8a-release.7z.004
```

All parts are sent to the specified Telegram chat.

## 📱 Extracting on Android

Download **all parts** into the same directory.

For example:

```text
apk-arm64-v8a-release.7z.001
apk-arm64-v8a-release.7z.002
apk-arm64-v8a-release.7z.003
apk-arm64-v8a-release.7z.004
```

Open:

```text
apk-arm64-v8a-release.7z.001
```

with **ZArchiver** and choose **Extract**.

ZArchiver will use the remaining volumes automatically as long as they are in the same directory.

You only need to start extraction from `.001`.

## 🔐 Security

The workflow uses:

- GitHub's `GITHUB_TOKEN` to access the artifact.
- `TELEGRAM_BOT_TOKEN` stored as a GitHub Actions secret.
- `chat_id` supplied as a workflow input.

The bot token must remain private.

Do not put it directly into:

- workflow YAML
- README files
- source code
- commit messages
- public issues

## ⚠️ Limitations

- The artifact must exist and must not be expired.
- The artifact name must match exactly.
- Each Telegram volume is approximately 45 MB.
- All volumes are required to reconstruct the archive.
- The standard Telegram Bot API currently supports document uploads up to 50 MB per file. citeturn0search0
- The workflow creates a multi-volume 7z archive rather than sending the original artifact ZIP directly.

## 🙏 Special Thanks

Special thanks to **ChatGPT** for helping design, troubleshoot, and refine
the GitHub Actions → Telegram artifact workflow.

> Replace `YOUR_NAME` with the GitHub username or name of the person who helped you.

## 📄 License

This project is licensed under the **MIT License**.

You are free to:

- use the project
- modify it
- distribute it
- use it in private or commercial projects

The copyright and license notice should be preserved with copies of the project.

See the [`LICENSE`](LICENSE) file for the complete license text.

## 🐢 Workflow Summary

```text
┌─────────────────────────────┐
│ GitHub Actions Run URL      │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│ Find artifact by name       │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│ Download artifact.zip       │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│ Extract ZIP                 │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│ Create 45 MB 7z volumes     │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│ Send parts to Telegram      │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│ ZArchiver → Extract .001    │
└─────────────────────────────┘
```
