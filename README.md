# Recall 🧠

**Persistent cross-session memory for Claude and other AI agents. Survives context limits, session restarts, and compaction.**

Store decisions, patterns, and knowledge once — retrieve them automatically across every future conversation. No more repeating yourself. No more losing critical context when the window fills up.

---

## Cloud vs. Self-Hosted

Recall comes in two flavours. Choose the one that fits your situation:

### ☁️ [recallmcp.com](https://recallmcp.com) — Managed SaaS (Recommended for most users)

Zero infrastructure. Sign up, get an API key, add one config block, done.

| What you get | Detail |
|---|---|
| **No Redis required** | Fully managed storage — nothing to install or maintain |
| **Up in 2 minutes** | One config block in Claude Desktop or Claude Code |
| **Team sharing** | One Claude learns something, all others know instantly |
| **Enterprise security** | AES-256-GCM encryption at rest, per-tenant key isolation |
| **Auto-consolidation** | Automatically clusters and merges similar memories |
| **Cross-session workflows** | Named threads that span multiple sessions |
| **7 embedding providers** | Voyage AI, Cohere, OpenAI, Deepseek, Grok, Anthropic, Ollama |
| **Pricing** | Free (500 memories) · Pro $4.99/mo (5K) · Team $9.99/mo (25K) |

```json
{
  "mcpServers": {
    "recall": {
      "url": "https://recallmcp.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

→ **[Get started at recallmcp.com](https://recallmcp.com)**

---

### 🖥️ This Repository — Self-Hosted (Redis or Valkey)

Run Recall entirely on your own infrastructure. Your data never leaves your servers.

| What you get | Detail |
|---|---|
| **Full data ownership** | Memories stored in your own Redis or Valkey instance |
| **No usage limits** | No memory caps, no subscription required |
| **Air-gapped deployments** | Works fully offline, no external API calls for storage |
| **Bring your own embeddings** | Configure any of 7 providers, or use the Anthropic keyword fallback |
| **Open source** | MIT licensed, fully auditable, fork-friendly |
| **Requires** | Redis ≥ 6 or Valkey, Node.js ≥ 18, your own server/VPS |

Best for: developers who need full control, on-premise requirements, or who already run Redis infrastructure.

---

## ⚠️ Security & Privacy Disclaimer

**IMPORTANT: READ BEFORE USE**

Recall stores conversation memories in either **Redis** or **Valkey**, which may contain sensitive information including:

- Code snippets, API keys, credentials, and secrets discussed in conversations
- Business logic, architecture decisions, and proprietary information
- Personal data, team member names, and organizational details
- Any other context shared with Claude during conversations

**You are responsible for:**

1. **Redis/Valkey Security**: Ensure your memory store is properly secured with authentication, TLS encryption, and network access controls  
2. **Data Ownership**: Only use Redis/Valkey servers that YOU control or have explicit permission to use  
3. **Access Control**: Understand who has access to your memory store and stored memories  
4. **Sensitive Data**: Never store memories on shared/public instances if they contain sensitive information  
5. **Compliance**: Ensure your use complies with your organization's data policies and relevant regulations (GDPR, CCPA, etc.)

**Disclaimer of Liability:**  
THIS SOFTWARE IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND. THE AUTHOR (JOSÉ AIROSA) IS NOT LIABLE FOR:

- Data breaches, leaks, or unauthorized access to stored memories  
- Loss of data or corrupted memories  
- Compliance violations or regulatory issues  
- Any damages arising from the use or misuse of this software

By using Recall, you acknowledge that you understand these risks and accept full responsibility for:

- Securing your Redis or Valkey infrastructure  
- Managing access to stored memories  
- Protecting sensitive information  
- Compliance with applicable laws and regulations

**Best Practices:**

- Use dedicated Redis or Valkey instances with strong authentication  
- Enable TLS/SSL encryption (`rediss://`) for remote connections  
- Regularly audit stored memories for sensitive data  
- Implement access controls and firewall rules  
- Use separate databases for different security contexts  
- Consider data retention policies and periodic cleanup  
- Never share connection strings publicly

---

## What is This?

Recall is a **brain extension** for Claude that stores memories persistently. It solves the context window problem by:

- 📝 **Remembering** directives, decisions, code patterns, and important information
- 🔍 **Retrieving** relevant context automatically when you need it
- 🔄 **Persisting** across sessions – memories survive restarts and context compaction
- 🗂️ **Organizing** by workspace – Project A memories don't pollute Project B

---

## Recall Cloud (Easiest Setup)

**No Redis setup required!** Get started in 2 minutes with the managed cloud service.

### 1. Sign Up

Go to **[recallmcp.com](https://recallmcp.com)** and create a free account.

### 2. Get Your API Key

1. Sign in to your dashboard at [recallmcp.com/dashboard](https://recallmcp.com/dashboard)
2. Go to **API Keys** section
3. Copy your API key (starts with `sk-recall-`)

### 3. Configure Claude Desktop

Add to your Claude Desktop configuration file:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`

**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "recall": {
      "url": "https://recallmcp.com/mcp",
      "headers": {
        "Authorization": "Bearer sk-recall-your-api-key-here"
      }
    }
  }
}
```

> **Note**: Replace `sk-recall-your-api-key-here` with your actual API key from the dashboard.

### 4. Restart Claude Desktop

Quit and reopen Claude Desktop to load the MCP server.

### 5. Test It

Ask Claude:

```text
"Store a memory that I prefer TypeScript for all projects"
```

Then in a new conversation:

```text
"What do you know about my coding preferences?"
```

**It remembers across all your devices!**

---

### Cloud Plans

| Plan       | Price    | Memories | Features                                  |
| ---------- | -------- | -------- | ----------------------------------------- |
| **Free**   | $0/mo    | 500      | 1 workspace, basic search                 |
| **Pro**    | $4.99/mo | 5,000    | 3 workspaces, API access, semantic search |
| **Team**   | $9.99/mo | 25,000   | Unlimited workspaces, shared memories     |

Upgrade anytime at [recallmcp.com/dashboard/billing](https://recallmcp.com/dashboard/billing)

---

### Claude Desktop Custom Connector (OAuth)

**Alternative Setup**: Claude Desktop supports native OAuth connections, so you don't need to edit config files manually.

#### 1. Open Claude Desktop Settings

1. Open Claude Desktop
2. Click on your profile icon → **Settings**
3. Go to **Integrations** → **Custom Connectors**

#### 2. Add Recall Connector

Click **"Add custom connector"** and enter:

| Field                      | Value                                  |
| -------------------------- | -------------------------------------- |
| **Name**                   | `Recall`                               |
| **Remote MCP server URL**  | `https://recallmcp.com/mcp`            |
| **OAuth Client ID**        | `claude-desktop`                       |
| **OAuth Client Secret**    | *(leave empty)*                        |

#### 3. Authenticate

1. Click **Add** to save the connector
2. Claude Desktop will open a browser window to **recallmcp.com**
3. Sign in with your Google or GitHub account
4. Grant access to connect your Recall account

#### 4. Start Using Recall

Once authenticated, Recall tools are automatically available in Claude Desktop. Ask Claude:

```text
"Store a memory that I prefer dark mode for all applications"
```

Your memories sync across all sessions and devices!

> **Note**: The OAuth connection refreshes automatically. If you see authentication errors, remove and re-add the connector.

---

## Self-Hosted Setup (5 Minutes)

### 1. Prerequisites

- **Redis** or **Valkey** running locally (`localhost:6379`)  
- **Node.js** 18 or higher  
- **Claude Code** or **Claude Desktop**

**Option A: Local Redis**

```bash
# macOS
brew install redis
brew services start redis

# Ubuntu/Debian
sudo apt-get install redis-server
sudo systemctl start redis

# Docker
docker run -d -p 6379:6379 redis:latest
```

**Option B: Local Valkey**

```bash
# macOS
brew install valkey
brew services start valkey

# Docker
docker run -d -p 6379:6379 valkey/valkey:latest
```

**Option C: Cloud Redis (No local install needed)**

Use a free Redis cloud service:

- **[Upstash](https://upstash.com/)** - Free tier with 500,000 commands/month
- **[Redis Cloud](https://redis.com/try-free/)** - Free 30MB database
- **[Railway](https://railway.app/)** - Free Redis with credit

Then use the provided connection URL in your config:

```json
{
  "env": {
    "REDIS_URL": "rediss://default:password@your-redis-host.com:6379"
  }
}
```

**Option D: Cloud Valkey (No local install needed)**

Most cloud providers offer Valkey services with free or low-cost trials available:

- **[AWS ElastiCache](https://aws.amazon.com/elasticache/)** - Use free tier to try it out
- **[AWS Free Tier](https://aws.amazon.com/free/)** - General AWS free tier options

### 2. Install

**Option A: Using Claude CLI (Recommended)**

```bash
npx @modelcontextprotocol/create-server @joseairosa/recall
```

Or use the MCP command directly in your Claude configuration (no installation needed):

```json
{
  "mcpServers": {
    "recall": {
      "command": "npx",
      "args": ["-y", "@joseairosa/recall"]
    }
  }
}
```

**Option B: Global installation**

```bash
npm install -g @joseairosa/recall
```

**Option C: From source**

```bash
git clone https://github.com/joseairosa/recall.git
cd recall
npm install
npm run build
```

### 3. Configure Claude

Add to your Claude configuration file:

**Claude Code** (`~/.config/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`):
```json
{
  "mcpServers": {
    "recall": {
      "command": "recall",
      "env": {
        "REDIS_URL": "redis://localhost:6379",
        "ANTHROPIC_API_KEY": "your-api-key-here"
      }
    }
  }
}
```
or for Valkey:
```json
{
  "mcpServers": {
    "recall": {
      "command": "recall",
      "env": {
        "BACKEND_TYPE":"valkey",
        "VALKEY_HOST":"localhost",
        "VALKEY_PORT":6379 ,
        "ANTHROPIC_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

**Claude Desktop** (`~/Library/Application Support/Claude/claude_desktop_config.json` on macOS):
```json
{
  "mcpServers": {
    "recall": {
      "command": "npx",
      "args": ["-y", "@joseairosa/recall"],
      "env": {
        "REDIS_URL": "redis://localhost:6379",
        "ANTHROPIC_API_KEY": "your-api-key-here"
      }
    }
  }
}
```
or for Valkey:
```json
{
  "mcpServers": {
    "recall": {
      "command": "recall",
      "env": {
        "BACKEND_TYPE":"valkey",
        "VALKEY_HOST":"localhost",
        "VALKEY_PORT":6379 ,
        "ANTHROPIC_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

### 4. Restart Claude

Restart Claude Code or Claude Desktop to load the MCP server.

### 5. Verify It Works

Ask Claude:

```text
"Store a memory that I prefer using TypeScript for all new projects"
```

Then in a new conversation:

```text
"What do you know about my coding preferences?"
```

**It remembers!**

---

## Claude Code Plugin (One-Liner Install)

Install the Recall plugin for Claude Code with a single command. This gives you enhanced commands, agents, and hooks for working with large files and RLM (Recursive Language Model) features.

### Quick Install

```bash
curl -fsSL https://raw.githubusercontent.com/joseairosa/recall/main/scripts/install-plugin.sh | bash
```

This script will:
- Download the Recall plugin to `~/.claude/plugins/recall-rlm/`
- Configure the MCP server in `~/.claude/settings.json`
- Support updates (re-run the same command to update)
- Create backups before updates

### After Installation

1. **Get your API key** from [recallmcp.com](https://recallmcp.com)

2. **Set the environment variable:**
   ```bash
   export RECALL_API_KEY=sk-recall-your-key-here

   # Add to your shell profile for persistence:
   echo 'export RECALL_API_KEY=sk-recall-your-key-here' >> ~/.zshrc
   ```

3. **Restart Claude Code** to load the plugin

### Plugin Features

The plugin adds:

| Feature | Description |
|---------|-------------|
| `/load-context <file>` | Load large files into RLM system for chunk-based analysis |
| `/decompose <chain_id>` | Break tasks into subtasks with strategies (filter, chunk, recursive) |
| `/rlm-status <chain_id>` | Check execution progress and results |

**Hooks (automatic behaviors):**
- Context injection before each prompt
- Large file detection (suggests RLM for >100KB files)
- Session summarization reminders
- Decision storage prompts

### Self-Hosted Configuration

If you prefer Redis locally instead of Recall Cloud, edit `~/.claude/settings.json` after installation:

```json
{
  "mcpServers": {
    "recall": {
      "command": "npx",
      "args": ["-y", "@joseairosa/recall"],
      "env": {
        "REDIS_URL": "redis://localhost:6379"
      }
    }
  }
}
```

### Update & Uninstall

```bash
# Update to latest version
curl -fsSL https://raw.githubusercontent.com/joseairosa/recall/main/scripts/install-plugin.sh | bash

# Uninstall
curl -fsSL https://raw.githubusercontent.com/joseairosa/recall/main/scripts/uninstall-plugin.sh | bash
```

---

## Upgrading

### Automatic Updates (Recommended)

If you use **`npx -y @joseairosa/recall`** in your config, you **automatically get the latest version** on every Claude restart. No action needed! 🎉

```json
{
  "mcpServers": {
    "recall": {
      "command": "npx",
      "args": ["-y", "@joseairosa/recall"]  // ← Always fetches latest
    }
  }
}
```

### Manual Updates

**If using global installation:**

```bash
# Update to latest version
npm update -g @joseairosa/recall

# Or install specific version
npm install -g @joseairosa/recall@1.3.0

# Check installed version
npm list -g @joseairosa/recall
```

**If using from source:**

```bash
cd recall
git pull origin main
npm install
npm run build
```

**After updating:**
1. Restart Claude Code or Claude Desktop
2. Verify new version: Ask Claude "What Recall version are you using?"
3. Check CHANGELOG.md for new features and breaking changes

### Version-Specific Upgrades

**Upgrading to v1.3.0:**
- No breaking changes, fully backward compatible
- To use global memories: Add `WORKSPACE_MODE=hybrid` to your config
- See [Migration Guide](#migrating-from-v121-to-v130) below

---

## How to Use

### ⚠️ Best Practices: Avoid Context Bloat

**Be selective with memory storage to keep Claude efficient.**

**Store HIGH-SIGNAL context:**
- ✅ Project preferences (coding style, tech stack, architecture patterns)
- ✅ Critical decisions ("We chose PostgreSQL over MongoDB because...")
- ✅ Important constraints (API limits, business rules, security requirements)
- ✅ Learned patterns from bugs/solutions

**Don't store LOW-SIGNAL content:**
- ❌ Code snippets (put those in files instead)
- ❌ Obvious facts or general knowledge
- ❌ Temporary context (only needed in current session)
- ❌ Duplicates of what's in documentation

**Keep memories concise:**
- ✅ Good: "API rate limit is 1000 req/min, prefer caching for frequent data"
- ❌ Bad: "Here's the entire implementation of our caching layer: [50 lines of code]"

**Remember:** Recall is for high-level context, not a code repository. Quality over quantity keeps your context window efficient.

---

### Store Important Information

Claude can automatically extract and store memories, or you can be explicit:

**Natural language:**
```
"Remember that our API base URL is https://api.example.com/v2"
```

```
"Store this as a directive: Always use functional components in React"
```

**What Claude does:** Uses the `store_memory` tool to save this information with appropriate context type and importance.

**Store globally (v1.3+):**
```
"Remember globally: I prefer TypeScript strict mode for all projects"
```

```
"Store this as a global preference: Always use ULIDs for database IDs"
```

**What Claude does:** Uses `store_memory` with `is_global: true` to make it accessible across all workspaces (requires `WORKSPACE_MODE=hybrid` or `global`).

### Recall Context

Claude automatically retrieves relevant memories, but you can also ask:

```
"What do you remember about our database schema?"
```

```
"Recall any decisions we made about error handling"
```

**What Claude does:** Uses `recall_relevant_context` or `search_memories` to find related information.

### Analyze Conversations

Before context compaction, preserve important details:

```
"Analyze our conversation and remember the important parts"
```

**What Claude does:** Uses `analyze_and_remember` to extract structured memories from the conversation.

### Organize Sessions

Group related work:

```
"Summarize this session and save it as 'Authentication Refactoring'"
```

**What Claude does:** Uses `summarize_session` to create a session snapshot with all relevant memories.

### Get Time Window Context (v1.6+)

Retrieve all memories from a specific time period:

```
"Give me the context for the last 2 hours"
```

```
"Show me everything from the last 30 minutes, formatted as markdown"
```

```
"Get all high-importance memories from the last hour, grouped by type"
```

**What Claude does:** Uses `get_time_window_context` to retrieve and format memories from the specified time window.

**Options:**
- Time specification: hours, minutes, or explicit timestamps
- Output formats: markdown (default), JSON, or plain text
- Grouping: chronological, by type, by importance, or by tags
- Filtering: minimum importance, specific context types

**Example outputs:**
- **Markdown**: Clean formatted context ready to paste into documentation
- **JSON**: Structured data for external processing
- **Text**: Simple plain text summary

Perfect for building context files from work sessions or exporting specific time periods.

### Convert Memories (v1.3+)

**Promote workspace memory to global:**
```
"Convert this memory to global: mem_abc123"
```

**What Claude does:** Uses `convert_to_global` to move the memory from workspace-specific to globally accessible.

**Convert back to workspace:**
```
"Convert this global memory to workspace-specific: mem_xyz789"
```

**What Claude does:** Uses `convert_to_workspace` to move the memory from global to current workspace.

### Link Related Memories (v1.4+)

**Create relationships between memories:**
```
"Link these two memories: pattern mem_abc123 is an example of implementation mem_xyz789"
```

**What Claude does:** Uses `link_memories` to create an `example_of` relationship between the memories.

**Find related memories:**
```
"Show me all memories related to mem_abc123"
```

**What Claude does:** Uses `get_related_memories` to traverse the relationship graph and find connected memories.

**Remove a relationship:**
```
"Unlink relationship rel_123abc"
```

**What Claude does:** Uses `unlink_memories` to remove the relationship while keeping both memories intact.

### Version History & Rollback (v1.5+)

**View version history:**
```
"Show me the version history for memory mem_abc123"
```

**What Claude does:** Uses `get_memory_history` to retrieve all versions with timestamps and change reasons.

**Rollback to previous version:**
```
"Rollback memory mem_abc123 to version ver_xyz789"
```

**What Claude does:** Uses `rollback_memory` to restore the memory to the specified version while preserving relationships.

### Memory Templates (v1.5+)

**Create a template:**
```
"Create a template for bug reports with fields: {{title}}, {{severity}}, {{steps}}"
```

**