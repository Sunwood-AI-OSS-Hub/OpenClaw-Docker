# clawd-multi-agent-discord-docker

Clawdbot を使用した **3つの独立した Discord ボット** を Docker Compose で運用するためのセットアップです。GLM-4.7 モデルを使用します。

## 概要

このプロジェクトでは、1つの GLM-4.7 API キーを共有しつつ、3つの独立した Discord ボットを立ち上げます。各ボットは独自のゲートウェイプロセスとコンテナで動作します。

### ボット構成

| ボット名 | Discord ID | ポート | 説明 |
|---------|-----------|--------|------|
| CL1-Kuroha | 1463974919137399000 | 18789 | Bot 1 |
| CL2-Reika  | 1463976407582511195 | 18791 | Bot 2 |
| CL3-Sentinel | 1463977421656162344 | 18793 | Bot 3 |

## アーキテクチャ

```
┌─────────────────────────────────────────────────────────────┐
│                      Docker Host                             │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │  clawdbot-   │  │  clawdbot-   │  │  clawdbot-   │      │
│  │    bot1      │  │    bot2      │  │    bot3      │      │
│  │  (CL1-Kuroha)│  │  (CL2-Reika) │  │ (CL3-Sentinel)│     │
│  │              │  │              │  │              │      │
│  │  Gateway     │  │  Gateway     │  │  Gateway     │      │
│  │  Port: 18789 │  │  Port: 18791 │  │  Port: 18793 │      │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘      │
│         │                 │                 │              │
│         └─────────────────┴─────────────────┘              │
│                         │                                   │
│                   ┌─────▼─────┐                             │
│                   │   GLM API  │                             │
│                   │  (共通使用)  │                             │
│                   └───────────┘                             │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
                   ┌───────────────┐
                   │  Discord API   │
                   │  (各ボットが   │
                   │   別セッション) │
                   └───────────────┘
```

## 前提条件

- Docker と Docker Compose v2 がインストールされていること
- [Zhipu AI GLM-4.7](https://open.bigmodel.cn/) の API キーを持っていること
- 3つの Discord ボットトークンを持っていること

## セットアップ手順

### 1. Discord ボットの作成

[Discord Developer Portal](https://discord.com/developers/applications) で3つのアプリケーションを作成し、それぞれのトークンを取得します。

**必須権限:**
- Message Content Intent
- Server Members Intent
- Presence Intent

### 2. リポジトリのクローンとビルド

```bash
# Clawdbot リポジトリをクローン
git clone https://github.com/clawdbot/clawdbot.git
cd clawdbot

# Docker イメージをビルド
docker build -t clawdbot:local .
```

### 3. プロジェクトのセットアップ

```bash
# プロジェクトディレクトリを作成
mkdir clawd-multi-agent-discord-docker
cd clawd-multi-agent-discord-docker

# 設定ファイルを作成（以下のセクションを参照）
```

### 4. 環境変数の設定

`.env` ファイルを作成:

```bash
# Gateway トークン生成 (32文字のランダムなhex)
# それぞれ別の値を生成してください
openssl rand -hex 32  # Bot 1 用
openssl rand -hex 32  # Bot 2 用
openssl rand -hex 32  #Bot 3 用
```

`.env` ファイル:

```bash
# =============================================================================
# Clawdbot - 3 Independent Bots
# =============================================================================

# Gateway tokens (run: openssl rand -hex 32)
CLAWDBOT_BOT1_GATEWAY_TOKEN=生成したトークン1
CLAWDBOT_BOT2_GATEWAY_TOKEN=生成したトークン2
CLAWDBOT_BOT3_GATEWAY_TOKEN=生成したトークン3

# GLM 4.7 API Key (共通)
GLM_API_KEY=あなたのGLM_APIキー

# Discord Bot Tokens (3 separate accounts)
DISCORD_BOT1_TOKEN=あなたのDiscord_Bot1トークン
DISCORD_BOT2_TOKEN=あなたのDiscord_Bot2トークン
DISCORD_BOT3_TOKEN=あなたのDiscord_Bot3トークン
```

### 5. 設定ファイルの作成

各ボットの設定ディレクトリ構造:

```
./
├── docker-compose.yml
├── .env
├── config/
│   ├── bot1/
│   │   ├── clawdbot.json
│   │   └── models.json
│   ├── bot2/
│   │   ├── clawdbot.json
│   │   └── models.json
│   └── bot3/
│       ├── clawdbot.json
│       └── models.json
└── workspace/
    ├── bot1/
    ├── bot2/
    └── bot3/
```

#### `config/bot*/models.json`:

```json
{
  "mode": "append",
  "providers": {
    "glm": {
      "baseUrl": "https://open.bigmodel.cn/api/paas/v4/",
      "apiKey": "あなたのGLM_APIキー",
      "api": "openai-completions",
      "models": [
        {
          "id": "glm-4.7",
          "name": "GLM-4.7"
        }
      ]
    }
  }
}
```

#### `config/bot*/clawdbot.json`:

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "glm/glm-4.7"
      }
    }
  },
  "channels": {
    "discord": {
      "enabled": true,
      "groupPolicy": "open"
    }
  },
  "gateway": {
    "mode": "local"
  },
  "messages": {
    "ackReactionScope": "all"
  }
}
```

### 6. Docker Compose の作成

`docker-compose.yml`:

```yaml
services:
  # Bot 1 - Gateway
  clawdbot-bot1:
    image: clawdbot:local
    container_name: clawdbot-bot1
    environment:
      HOME: /home/node
      TERM: xterm-256color
      CLAWDBOT_GATEWAY_TOKEN: ${CLAWDBOT_BOT1_GATEWAY_TOKEN}
      GLM_API_KEY: ${GLM_API_KEY}
      DISCORD_BOT_TOKEN: ${DISCORD_BOT1_TOKEN}
    volumes:
      - ./config/bot1:/home/node/.clawdbot
      - ./workspace/bot1:/home/node/clawd
    ports:
      - "18789:18789"
    init: true
    restart: unless-stopped
    command: ["node", "dist/index.js", "gateway"]
    networks:
      - clawdbot-network

  # Bot 2 - Gateway
  clawdbot-bot2:
    image: clawdbot:local
    container_name: clawdbot-bot2
    environment:
      HOME: /home/node
      TERM: xterm-256color
      CLAWDBOT_GATEWAY_TOKEN: ${CLAWDBOT_BOT2_GATEWAY_TOKEN}
      GLM_API_KEY: ${GLM_API_KEY}
      DISCORD_BOT_TOKEN: ${DISCORD_BOT2_TOKEN}
    volumes:
      - ./config/bot2:/home/node/.clawdbot
      - ./workspace/bot2:/home/node/clawd
    ports:
      - "18791:18789"
    init: true
    restart: unless-stopped
    command: ["node", "dist/index.js", "gateway"]
    networks:
      - clawdbot-network

  # Bot 3 - Gateway
  clawdbot-bot3:
    image: clawdbot:local
    container_name: clawdbot-bot3
    environment:
      HOME: /home/node
      TERM: xterm-256color
      CLAWDBOT_GATEWAY_TOKEN: ${CLAWDBOT_BOT3_GATEWAY_TOKEN}
      GLM_API_KEY: ${GLM_API_KEY}
      DISCORD_BOT_TOKEN: ${DISCORD_BOT3_TOKEN}
    volumes:
      - ./config/bot3:/home/node/.clawdbot
      - ./workspace/bot3:/home/node/clawd
    ports:
      - "18793:18789"
    init: true
    restart: unless-stopped
    command: ["node", "dist/index.js", "gateway"]
    networks:
      - clawdbot-network

  # CLI for setup
  clawdbot-cli:
    image: clawdbot:local
    container_name: clawdbot-cli
    environment:
      HOME: /home/node
      TERM: xterm-256color
      BROWSER: echo
      GLM_API_KEY: ${GLM_API_KEY}
    volumes:
      - ./config/bot1:/home/node/.clawdbot
      - ./workspace/bot1:/home/node/clawd
    stdin_open: true
    tty: true
    init: true
    entrypoint: ["node", "dist/index.js"]
    profiles:
      - cli
    networks:
      - clawdbot-network

networks:
  clawdbot-network:
    driver: bridge
```

### 7. Discord チャンネルの追加

初回起動前に、各ボットで Discord チャンネルを追加する必要があります:

```bash
# Bot 1
docker compose --profile cli run --rm clawdbot-cli \
    channels add --channel discord --token "${DISCORD_BOT1_TOKEN}"

# bot2 用に config を切り替えてから、Bot 2 用のコマンドを実行
# (CLI コンテナの volumes を変更して再実行)
```

または、CLI コンテナの volumes を一時的に変更して各ボットに設定します。

### 8. ボットの起動

```bash
# すべてのボットを起動
docker compose up -d

# ステータス確認
docker compose ps

# ログ確認
docker compose logs -f
```

## 設定項目の解説

### clawdbot.json

| 項目 | 値 | 説明 |
|------|-----|------|
| `agents.defaults.model.primary` | `glm/glm-4.7` | 使用するモデル |
| `channels.discord.enabled` | `true` | Discord を有効化 |
| `channels.discord.groupPolicy` | `open` | すべてのチャンネルで応答 |
| `messages.ackReactionScope` | `all` | すべてのメッセージにリアクション |

- **groupPolicy**:
  - `open`: すべてのチャンネルで応答
  - `allowlist`: 許可されたチャンネルのみで応答

- **ackReactionScope**:
  - `all`: すべてのメッセージにリアクション
  - `group-mentions`: メンションのみにリアクション

### models.json

| 項目 | 値 | 説明 |
|------|-----|------|
| `mode` | `append` | 既存プロバイダーに追加 |
| `providers.glm.baseUrl` | GLM API endpoint | Zhipu AI の API URL |
| `providers.glm.api` | `openai-completions` | OpenAI 互換モード |

## コマンド

### ボットの操作

```bash
# 起動
docker compose up -d

# 停止
docker compose down

# 再起動
docker compose restart

# 特定のボットのみ再起動
docker compose restart clawdbot-bot1

# ログを確認
docker compose logs -f clawdbot-bot1

# すべてのログを確認
docker compose logs -f
```

### ボットの設定変更

```bash
# CLI で設定変更
docker compose --profile cli run --rm clawdbot-cli

# コンテナ内でコマンド実行
docker exec -it clawdbot-bot1 node dist/index.js config set ...
```

## トラブルシューティング

### 「Unknown model: glm/glm-4.7」エラー

**原因:** `models.json` が正しく設定されていない、または API キーが正しくない

**解決策:**
1. `models.json` で `apiKey` が正しく設定されているか確認
2. API キーが有効か確認
3. GLM API にアクセスできるか確認

### ボットがオフライン表示

**原因:** Discord トークンが間違っている、または権限が不足している

**解決策:**
1. Discord トークンが `.env` に正しく設定されているか確認
2. Message Content Intent が有効になっているか確認
3. ボットがサーバーに招待されているか確認

### 「gateway already running」エラー

**原因:** 古いプロセスが残っている

**解決策:**
```bash
docker compose down
docker compose up -d
```

### ポート競合エラー

**原因:** ポート 18789, 18791, 18793 が使用中

**解決策:**
```bash
# 使用中のプロセスを確認
sudo lsof -i :18789
sudo lsof -i :18791
sudo lsof -i :18793

# プロセスを終了
sudo kill -9 <PID>
```

### リアクションがつかない

**原因:** `ackReactionScope` の設定

**解決策:**
`config/bot*/clawdbot.json` で以下を確認:
```json
{
  "messages": {
    "ackReactionScope": "all"
  }
}
```

## 参考リンク

- [Clawdbot ドキュメント](https://docs.clawd.bot)
- [Clawdbot GitHub](https://github.com/clawdbot/clawdbot)
- [Zhipu AI GLM API](https://open.bigmodel.cn/)
- [Discord Developer Portal](https://discord.com/developers/applications)

## ライセンス

このプロジェクトは Clawdbot のセットアップ例です。
Clawdbot のライセンスに従ってください。
