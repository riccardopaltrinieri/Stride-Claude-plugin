# Stride Claude Code Plugin

Connects [Claude Code](https://claude.ai/code) to the [Stride](https://stride-app.com) MCP server using end-user OAuth. No secrets, no scripts, no dependencies — authentication is handled entirely by Claude Code's built-in browser login flow.

## What this plugin does

- Registers the Stride MCP server (`https://mcp.strideapp.coach`) with Claude Code
- Triggers Claude Code's built-in OAuth browser flow on first use (via Logto)
- Stores tokens locally in Claude Code — nothing is ever committed to this repo

---

## Installation

### Option 1: Install from git URL (recommended)

```bash
claude mcp add --transport http https://mcp.strideapp.coach \
  --name stride \
  --oauth-client-id YOUR_LOGTO_CLIENT_ID
```

Or install the plugin directly from this repo:

```bash
claude plugin install https://github.com/riccardopaltrinieri/Stride-Claude-plugin
```

### Option 2: Add as a marketplace plugin

1. Open Claude Code
2. Go to **Settings → Plugins → Browse Marketplace** (or run `/plugins` in the chat)
3. Search for **Stride** and click **Install**

### Option 3: Manual setup (copy `.mcp.json`)

Copy `.mcp.json` from this repo into your project root (or your `~/.claude/` directory for global access), then replace `YOUR_LOGTO_CLIENT_ID` with the actual client ID.

---

## First use — browser login flow

The first time Claude Code connects to the Stride MCP server, it will:

1. Detect that OAuth is required
2. Open a browser window to the Logto login page (`https://mqj2ap.logto.app/oidc`)
3. Ask you to log in with your Stride account
4. Redirect back to Claude Code and store the access token locally

After that, Claude Code silently refreshes tokens in the background. You won't need to log in again unless your session expires or you explicitly sign out.

---

## Re-authenticating (if your token expires)

If Stride tools stop working or Claude Code reports an authentication error:

1. Run `/mcp` in the Claude Code chat
2. Find **stride** in the server list
3. Click **Reconnect** or **Re-authenticate**
4. A new browser login flow will start automatically

---

## Security

- **No secrets in this repo.** The `clientId` in `.mcp.json` is a public identifier — it is safe to commit.
- **No token scripts.** Claude Code handles token acquisition, storage, and refresh natively.
- **PKCE only.** All OAuth flows use PKCE, preventing authorization code interception attacks.
- Tokens are stored in Claude Code's local credential store, never in project files.
