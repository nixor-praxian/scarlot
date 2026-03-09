# Setup

How to get the Scarlot bot running on your machine or a VPS.

## Prerequisites

- Node.js 20+
- Docker
- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) with an active subscription or API key
- A WhatsApp account you can link a device to

## 1. Clone both repos

```bash
git clone https://github.com/nixor-praxian/scarlot.git
git clone https://github.com/nichochar/nanoclaw.git
```

They should be side by side:

```
~/projects/           # or wherever
├── scarlot/          # this repo — project docs, bot personality, config
└── nanoclaw/         # the engine — WhatsApp connection, message routing, containers
```

## 2. Set up NanoClaw

```bash
cd nanoclaw
claude
```

Then run `/setup` inside Claude Code. It walks you through:

1. Installing dependencies
2. Building the container image
3. Authenticating WhatsApp (QR code scan with your phone)
4. Registering groups

## 3. Register the Scarlot group

Once NanoClaw is running, register a group for Scarlot. In the NanoClaw admin chat (your self-chat on WhatsApp), ask the bot to add a group with the scarlot repo mounted.

Or manually edit `data/registered_groups.json`:

```json
{
  "YOUR_GROUP_JID": {
    "name": "Scarlot HQ",
    "folder": "scarlot-hq",
    "trigger": "@Scarlot",
    "requiresTrigger": false,
    "added_at": "2026-03-09T00:00:00Z",
    "containerConfig": {
      "additionalMounts": [
        {
          "hostPath": "~/projects/scarlot",
          "containerPath": "scarlot",
          "readonly": false
        }
      ]
    }
  }
}
```

Then copy the bot personality into the NanoClaw group folder:

```bash
cp scarlot/claw/CLAUDE.md nanoclaw/groups/scarlot-hq/CLAUDE.md
```

Or symlink it so changes stay in sync:

```bash
ln -s ~/projects/scarlot/claw/CLAUDE.md nanoclaw/groups/scarlot-hq/CLAUDE.md
```

## 4. Start

```bash
cd nanoclaw
npm run dev
```

The bot is live. Messages to your WhatsApp number are handled by the agent using the personality defined in `scarlot/claw/CLAUDE.md`.

## VPS deployment

For running on a small VPS (Ubuntu recommended):

```bash
# Install dependencies
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo bash -
sudo apt-get install -y nodejs docker.io
sudo systemctl enable docker
sudo usermod -aG docker $USER

# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Clone repos
git clone https://github.com/nixor-praxian/scarlot.git
git clone https://github.com/nichochar/nanoclaw.git

# Set up NanoClaw
cd nanoclaw
claude    # then run /setup
```

For WhatsApp auth on a headless VPS, NanoClaw supports pairing codes (no QR scanning needed). The `/setup` skill handles this.

To keep it running after SSH disconnect:

```bash
# Option 1: systemd (NanoClaw's /setup can configure this)
# Option 2: tmux
tmux new -s scarlot
npm run dev
# Ctrl+B, D to detach
```

## Environment variables

NanoClaw needs either:

- `CLAUDE_CODE_OAUTH_TOKEN` — from a Claude Code subscription, or
- `ANTHROPIC_API_KEY` — direct API access

Set these in your shell profile or a `.env` file in the nanoclaw directory.
