<img src="https://raw.githubusercontent.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/main/assets/release-header-v0.2.1.svg" alt="OpenClaw v0.2.1 Release"/>

# v0.2.1 - Rename Refinement / 名称変更の細部修正

**Release Date / リリース日:** 2026-02-02

---

## Japanese / 日本語

### 概要

v0.2.1 は、**Clawdbot から OpenClaw への移行**における細かい修正を含むマイナーリリースです。

### 主な変更点

- **名称変更の完全化**: 残存していた clawdbot 関連ファイル・設定を openclaw に変更
- **Docker Compose 構成の更新**: マルチBot構成のリネーム対応
- **Fly.io デプロイスクリプト更新**: `fly-deploy-openclaw.sh` に名称変更

---

### 変更 (🔄 Changes)

#### リネーム修正
- **clawdbot → openclaw 完全移行** ([#33](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/33))
  - config/bot3 内の設定ファイル名を変更 (`clawdbot.json` → `openclaw.json`)
  - config/flyio 内の設定ファイル名を変更
  - Docker Compose ファイル内のサービス名・イメージ名を更新
  - Fly.io デプロイスクリプトを `fly-deploy-openclaw.sh` に改名
  - `.gitmodules` のサブモジュールパスを更新

#### ドキュメント更新
- **README 更新** ([#33](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/33))
  - clawdbot から openclaw への記載を完全に移行

#### 設定ファイル
- **環境変数ファイル更新**
  - `.env.example` / `.env.fly.example` 内の記載を更新

#### CI/CD
- **GitHub Actions 更新**
  - Dockerビルドワークフロー内のパス・イメージ名を更新

---

### アップグレード方法

```bash
# v0.2.1 を取得
git fetch --tags
git checkout v0.2.1

# サブモジュールを更新
git submodule update --init --recursive

# Docker イメージをビルド/プル
docker-compose pull
```

---

## English

### Overview

v0.2.1 is a minor release containing refinements for the **Clawdbot to OpenClaw migration**.

### Key Changes

- **Complete rename transition**: Remaining clawdbot-related files and configurations updated to openclaw
- **Docker Compose updates**: Multi-bot configuration rename support
- **Fly.io deploy script update**: Renamed to `fly-deploy-openclaw.sh`

---

### Changes (🔄 Changes)

#### Rename Refinement
- **clawdbot → openclaw complete transition** ([#33](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/33))
  - Renamed config files in config/bot3 (`clawdbot.json` → `openclaw.json`)
  - Renamed config files in config/flyio
  - Updated service names and image names in Docker Compose files
  - Renamed Fly.io deploy script to `fly-deploy-openclaw.sh`
  - Updated submodule path in `.gitmodules`

#### Documentation Updates
- **README updates** ([#33](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/33))
  - Complete transition from clawdbot to openclaw in documentation

#### Configuration Files
- **Environment variable file updates**
  - Updated references in `.env.example` / `.env.fly.example`

#### CI/CD
- **GitHub Actions updates**
  - Updated paths and image names in Docker build workflow

---

### Upgrade Instructions

```bash
# Get v0.2.1
git fetch --tags
git checkout v0.2.1

# Update submodules
git submodule update --init --recursive

# Pull Docker images
docker-compose pull
```

---

## Full Changelog

[Compare v0.2.0...v0.2.1](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/compare/v0.2.0...v0.2.1)

---

## Contributors

**@Sunwood-AI-OSS-Hub** and contributors

---

## Docker Images

| Image | Tags | Architectures |
|:------|:-----|:--------------|
| `ghcr.io/sunwood-ai-oss-hub/openclaw` | `latest`, `v0.2.1` | linux/amd64, linux/arm64 |
| `ghcr.io/sunwood-ai-oss-hub/openclaw-browser` | `latest`, `v0.2.1` | linux/amd64, linux/arm64 |
| `ghcr.io/sunwood-ai-oss-hub/openclaw-infinity` | `latest`, `v0.2.1` | linux/amd64, linux/arm64 |
