<img src="https://raw.githubusercontent.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/main/assets/a-professional-minimalist-typography-log_mtsilabqRSGEVQ93r_sUcA_RauByotrRsCRIdwDGusbRg.jpeg" alt="OpenClaw v0.2.0 Release"/>

# v0.2.0 - Multi-Architecture & Deployment Enhancement / マルチアーキテクチャ対応とデプロイ機能強化

**Release Date / リリース日:** 2025-02-02

---

## Japanese / 日本語

### 概要

v0.2.0 は、**Clawd Multi-Agent Discord Docker** の主要な機能強化リリースです。Dockerイメージのマルチアーキテクチャ対応、Fly.io デプロイメントサポート、CI/CD ワークフローの導入、そして **Clawdbot から OpenClaw への名称変更**を含む多くの改善が含まれています。

### 主な変更点

- **名称変更**: Clawdbot → **OpenClaw**
- **マルチアーキテクチャ対応**: AMD64、ARM64 アーキテクチャをサポート
- **Fly.io デプロイ**: クラウドデプロイメントを完全サポート
- **GitHub Actions CI/CD**: 自動ビルド・デプロイワークフローを追加

---

### 新機能 (✨ Features)

#### Docker & CI/CD
- **マルチアーキテクチャ対応** ([#28](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/28), [#30](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/30))
  - AMD64/ARM64 アーキテクチャをサポート
  - GHCR (GitHub Container Registry) にイメージを公開
  - イメージ名: `ghcr.io/sunwood-ai-oss-hub/openclaw-*`

- **GitHub Actions CI/CD** ([#5](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/5), [#30](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/30))
  - 自動Dockerイメージビルドワークフロー
  - PR自動コメント機能
  - Claude GLM Responder ワークフロー

#### Fly.io デプロイメント
- **Fly.io デプロイサポート** ([#12](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/12), [#14](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/14), [#16](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/16), [#20](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/20), [#21](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/21))
  - Fly.io 用設定テンプレート
  - デプロイスクリプト (`scripts/fly-deploy-clawdbot.sh`)
  - IP制限機能
  - メモリサイズ2048MBに増量

#### 設定 & 環境変数
- **OpenRouter & ZAI API対応** ([#16](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/16), [#24](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/24))
  - OpenRouter API Key 環境変数サポート
  - ZAI API Key 環境変数サポート
  - fallbackモデル設定

- **マルチBot設定** ([#24](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/24))
  - Bot 2 & 3 用設定ファイル
  - Infinity版設定ファイル
  - 設定ファイルコピー機能

#### Docker Compose 分割
- **Docker Compose 柔軟対応** ([#5](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/5), [#9](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/9))
  - `docker-compose.yml` - 基本構成
  - `docker-compose.infinity.yml` - Infinity版
  - `docker-compose.multi.yml` - マルチBot版
  - `docker-compose.infinity.multi.yml` - Infinity マルチBot版

---

### バグ修正 (🐛 Bug Fixes)

- **Docker Hub から GHCR へ移行** ([#32](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/32))
  - 誤プッシュを修正 - GHCRのみ使用

- **ユーザー権限問題** ([#20](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/20))
  - agentユーザーの権限問題を修正

- **npm install 順序** ([#32](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/32))
  - browserでnpm installする前にrootユーザーに戻す

- **Playwright インストール** ([#32](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/32))
  - Playwrightをインストールしてからブラウザをインストール

- **OpenRouter環境変数** ([#25](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/25))
  - OpenRouter環境変数とnetworks設定を修正

---

### 変更 (🔄 Changes)

#### リファクタリング
- **ベースイメージ構造再編** ([#32](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/32))
  - `agentos-openclaw-base` ベースイメージ導入
  - 共通レイヤーの最適化

- **Main Bot設定簡素化** ([#24](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/24))
  - 設定ファイル構造を簡素化

- **Dockerfile ルート配置** ([#18](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/18))
  - メインDockerfileをルートディレクトリに移動

#### 名称変更
- **Clawdbot → OpenClaw** ([#26](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/26), [#27](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/27))
  - プロジェクト名称を OpenClaw に変更
  - ドキュメント・設定ファイルを更新

#### Git Submodule
- **Clawdbot サブモジュール化** ([#2](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/2))
  - clawdbot を Git submodule として統合
  - `--recursive` オプションでのクローンが必要

---

### ドキュメント (📝 Documentation)

- **README 日本語・英語対応** ([#1](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/1), [#27](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/27))
  - Docker Compose 分割手順
  - OpenRouter 設定方法
  - Infinity版起動手順
  - マルチアーキテクチャ対応情報

- **Fly.io デプロイドキュメント** ([#12](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/12), [#22](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/22))
  - fallbackモデル設定
  - IP制限設定

---

### その他 (♻️ Other)

- **Remotion スキル追加** ([#7](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/7))
- **agentos-clawd-agent3 イメージエイリアス** ([#5](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/5))
- **.dockerignore 追加** ([#18](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/18))
- **シークレットファイル除外** ([#12](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/12))

---

### アップグレード方法

```bash
# v0.2.0 を取得
git fetch --tags
git checkout v0.2.0

# サブモジュールを初期化
git submodule update --init --recursive

# Docker イメージをプル
docker-compose pull
```

**重要**: Git clone する場合は `--recursive` オプションを使用してください：
```bash
git clone --recursive https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker.git
```

---

## English

### Overview

v0.2.0 is a major feature enhancement release for **Clawd Multi-Agent Discord Docker**. It includes multi-architecture Docker image support, Fly.io deployment capabilities, CI/CD workflows, and the **rename from Clawdbot to OpenClaw**.

### Key Changes

- **Rename**: Clawdbot → **OpenClaw**
- **Multi-architecture support**: AMD64, ARM64 architectures
- **Fly.io deployment**: Full cloud deployment support
- **GitHub Actions CI/CD**: Automated build & deploy workflows

---

### What's New (✨ Features)

#### Docker & CI/CD
- **Multi-architecture support** ([#28](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/28), [#30](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/30))
  - Support for AMD64/ARM64 architectures
  - Images published to GHCR (GitHub Container Registry)
  - Image name: `ghcr.io/sunwood-ai-oss-hub/openclaw-*`

- **GitHub Actions CI/CD** ([#5](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/5), [#30](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/30))
  - Automated Docker image build workflows
  - PR auto-comment functionality
  - Claude GLM Responder workflow

#### Fly.io Deployment
- **Fly.io deployment support** ([#12](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/12), [#14](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/14), [#16](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/16), [#20](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/20), [#21](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/21))
  - Fly.io configuration templates
  - Deployment script (`scripts/fly-deploy-clawdbot.sh`)
  - IP restriction functionality
  - Memory increased to 2048MB

#### Configuration & Environment
- **OpenRouter & ZAI API support** ([#16](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/16), [#24](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/24))
  - OpenRouter API Key environment variables
  - ZAI API Key environment variables
  - Fallback model configuration

- **Multi-Bot configuration** ([#24](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/24))
  - Bot 2 & 3 configuration files
  - Infinity edition configuration files
  - Configuration file copy functionality

#### Docker Compose Split
- **Flexible Docker Compose** ([#5](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/5), [#9](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/9))
  - `docker-compose.yml` - Basic configuration
  - `docker-compose.infinity.yml` - Infinity edition
  - `docker-compose.multi.yml` - Multi-bot edition
  - `docker-compose.infinity.multi.yml` - Infinity multi-bot edition

---

### Bug Fixes (🐛 Bug Fixes)

- **Migrate from Docker Hub to GHCR** ([#32](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/32))
  - Fixed incorrect push - GHCR only

- **User permission issues** ([#20](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/20))
  - Fixed agent user permission problems

- **npm install order** ([#32](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/32))
  - Switch to root user before npm install in browser

- **Playwright installation** ([#32](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/32))
  - Install Playwright before installing browsers

- **OpenRouter environment variables** ([#25](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/25))
  - Fixed OpenRouter environment variables and networks configuration

---

### Changes (🔄 Changes)

#### Refactoring
- **Base image structure reorganization** ([#32](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/32))
  - Introduced `agentos-openclaw-base` base image
  - Optimized common layers

- **Main Bot configuration simplification** ([#24](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/24))
  - Simplified configuration file structure

- **Dockerfile root placement** ([#18](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/18))
  - Moved main Dockerfile to root directory

#### Rename
- **Clawdbot → OpenClaw** ([#26](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/26), [#27](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/27))
  - Renamed project to OpenClaw
  - Updated documentation and configuration files

#### Git Submodule
- **Clawdbot as submodule** ([#2](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/2))
  - Integrated clawdbot as Git submodule
  - Requires `--recursive` option for cloning

---

### Documentation (📝 Documentation)

- **README Japanese & English** ([#1](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/1), [#27](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/27))
  - Docker Compose split procedures
  - OpenRouter configuration guide
  - Infinity edition startup procedures
  - Multi-architecture support information

- **Fly.io deployment documentation** ([#12](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/12), [#22](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/22))
  - Fallback model configuration
  - IP restriction settings

---

### Other (♻️ Other)

- **Remotion skill added** ([#7](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/7))
- **agentos-clawd-agent3 image alias** ([#5](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/5))
- **.dockerignore added** ([#18](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/18))
- **Secret files exclusion** ([#12](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/pull/12))

---

### Upgrade Instructions

```bash
# Get v0.2.0
git fetch --tags
git checkout v0.2.0

# Initialize submodules
git submodule update --init --recursive

# Pull Docker images
docker-compose pull
```

**Important**: When cloning, use the `--recursive` option:
```bash
git clone --recursive https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker.git
```

---

## Full Changelog

[Compare v0.1.0...v0.2.0](https://github.com/Sunwood-AI-OSS-Hub/clawd-multi-agent-discord-docker/compare/v0.1.0...v0.2.0)

---

## Contributors

**@Sunwood-AI-OSS-Hub** and contributors

---

## Docker Images

| Image | Tags | Architectures |
|:------|:-----|:--------------|
| `ghcr.io/sunwood-ai-oss-hub/openclaw` | `latest`, `v0.2.0` | linux/amd64, linux/arm64 |
| `ghcr.io/sunwood-ai-oss-hub/openclaw-browser` | `latest`, `v0.2.0` | linux/amd64, linux/arm64 |
| `ghcr.io/sunwood-ai-oss-hub/openclaw-infinity` | `latest`, `v0.2.0` | linux/amd64, linux/arm64 |
