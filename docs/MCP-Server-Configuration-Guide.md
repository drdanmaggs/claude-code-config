# Claude Code MCP Server Configuration Guide

This guide provides practical, copy-paste ready configurations for connecting existing MCP servers to Claude Code (not Claude Desktop). All examples are tested and ready for immediate use.

## Quick Start Commands

### Add servers using Claude Code CLI
```bash
# Basic syntax
claude mcp add <server-name> <command> [args...]

# Examples
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem ~/Documents
claude mcp add github -e GITHUB_PERSONAL_ACCESS_TOKEN=ghp_token -- npx -y @modelcontextprotocol/server-github
claude mcp add notion -s project -- npx -y @notionhq/notion-mcp-server
```

### Scope options explained
- `-s local` (default): Current project only
- `-s project`: Shared via `.mcp.json` file  
- `-s user`: Available across all projects

## Configuration File Locations

**Claude Code** uses different configuration files than Claude Desktop:
- **User config**: `~/.claude.json` (all platforms)
- **Project config**: `.mcp.json` (in project root)
- **Settings**: `~/.claude/settings.json` (user) or `.claude/settings.json` (project)

## Popular MCP Server Configurations

### 1. Filesystem Server

Access local files and directories.

**Command line setup:**
```bash
claude mcp add filesystem -s user -- npx -y @modelcontextprotocol/server-filesystem ~/Desktop ~/Documents ~/Projects
```

**Direct configuration (.claude.json):**
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/username/Desktop",
        "/Users/username/Documents"
      ]
    }
  }
}
```

**Windows configuration:**
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:\\Users\\username\\Desktop",
        "C:\\Users\\username\\Documents"
      ]
    }
  }
}
```

### 2. GitHub Server

Manage repositories, issues, and pull requests.

**Command line setup:**
```bash
claude mcp add github -e GITHUB_PERSONAL_ACCESS_TOKEN=ghp_your_token -- npx -y @modelcontextprotocol/server-github
```

**Direct configuration:**
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_token_here"
      }
    }
  }
}
```

**Setup steps:**
1. Go to GitHub Settings → Developer settings → Personal access tokens
2. Create token with `repo`, `issues`, `pull_requests` permissions
3. Replace `ghp_your_token_here` with your actual token

### 3. Notion Server

Access and manage Notion pages and databases.

**Command line setup:**
```bash
claude mcp add notion -s project -- npx -y @notionhq/notion-mcp-server
```

**Direct configuration:**
```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer ntn_your_token\", \"Notion-Version\": \"2022-06-28\"}"
      }
    }
  }
}
```

**Setup steps:**
1. Create integration at https://www.notion.so/my-integrations
2. Copy integration token (starts with `ntn_`)
3. Share pages/databases with the integration
4. Replace `ntn_your_token` with your actual token

### 4. Supabase Server

Manage Supabase projects and databases.

**Command line setup:**
```bash
claude mcp add supabase -e SUPABASE_ACCESS_TOKEN=your_token -- npx -y @supabase/mcp-server-supabase@latest
```

**Direct configuration:**
```json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": [
        "-y",
        "@supabase/mcp-server-supabase@latest",
        "--access-token",
        "your-personal-access-token"
      ]
    }
  }
}
```

**Project-scoped configuration:**
```json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": [
        "-y",
        "@supabase/mcp-server-supabase@latest",
        "--access-token",
        "your-token",
        "--project-ref",
        "your-project-id"
      ]
    }
  }
}
```

### 5. Additional Popular Servers

**Memory Server** (persistent memory across sessions):
```bash
claude mcp add memory -- npx -y @modelcontextprotocol/server-memory
```

**Brave Search** (web search):
```bash
claude mcp add brave-search -e BRAVE_API_KEY=your_key -- npx -y @modelcontextprotocol/server-brave-search
```

**Puppeteer** (browser automation):
```bash
claude mcp add puppeteer -- npx -y @modelcontextprotocol/server-puppeteer
```

**Sequential Thinking** (structured reasoning):
```bash
claude mcp add sequential-thinking -- npx -y mcp-sequentialthinking-tools
```

**PostgreSQL** (database access):
```bash
claude mcp add postgres -- npx -y @modelcontextprotocol/server-postgres postgresql://localhost/mydb
```

## Project Configuration (.mcp.json)

Create `.mcp.json` in your project root for team-shared configurations:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "${workspaceFolder}/src",
        "${workspaceFolder}/docs"
      ]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

## Complete Multi-Server Example

Here's a comprehensive `.claude.json` configuration with multiple servers:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/username/Projects",
        "/Users/username/Documents"
      ]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_token"
      }
    },
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer ntn_your_token\", \"Notion-Version\": \"2022-06-28\"}"
      }
    },
    "supabase": {
      "command": "npx",
      "args": [
        "-y",
        "@supabase/mcp-server-supabase@latest",
        "--access-token",
        "your-supabase-token"
      ]
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

## Claude Code vs Claude Desktop Key Differences

| Feature | Claude Code | Claude Desktop |
|---------|-------------|----------------|
| Config location | `~/.claude.json` | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Project configs | `.mcp.json` supported | Not supported |
| CLI commands | Full `claude mcp` suite | Manual file editing only |
| Scope system | 3-level (local/project/user) | Global only |
| Team sharing | Via `.mcp.json` in version control | Manual sharing |

## Managing Servers

### List all configured servers
```bash
claude mcp list
```

### Check specific server details
```bash
claude mcp get filesystem
```

### Remove a server
```bash
claude mcp remove server-name
```

### Import from Claude Desktop
```bash
claude mcp add-from-claude-desktop
```

### Debug MCP connections
```bash
claude --mcp-debug
```

### Check server status (inside Claude Code)
```
/mcp
```

## Environment Variables

### Method 1: Direct in configuration
```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "server-package"],
      "env": {
        "API_KEY": "your-key",
        "OTHER_VAR": "value"
      }
    }
  }
}
```

### Method 2: Via command line
```bash
claude mcp add server -e API_KEY=value -e OTHER_VAR=value2 -- npx -y server-package
```

### Method 3: System environment reference
```json
{
  "mcpServers": {
    "server-name": {
      "command": "bash",
      "args": ["-c", "API_KEY=$MY_SYSTEM_VAR npx -y server-package"]
    }
  }
}
```

## Troubleshooting

### Server not connecting
1. Check paths are absolute, not relative
2. Verify Node.js is installed (v16+)
3. Test server manually: `npx -y @modelcontextprotocol/server-name`
4. Check logs: `~/Library/Logs/Claude/mcp*.log` (macOS)

### Permission errors
- Use `npx` instead of global installs
- Ensure Claude Code has file access permissions
- Verify API tokens have required scopes

### JSON syntax errors
- Use a JSON validator
- Escape quotes with `\"`
- Remove trailing commas

### Windows-specific issues
- Use double backslashes `\\` in paths
- Consider using WSL for better compatibility
- Full path to node.exe may be required

## Best Practices

1. **Use appropriate scopes**: User scope for personal tools, project scope for team tools
2. **Version control**: Include `.mcp.json` in git for team sharing
3. **Security**: Never hardcode sensitive tokens in shared files
4. **Naming**: Use descriptive server names in camelCase
5. **Testing**: Test servers manually before adding to Claude Code
6. **Documentation**: Document custom server configurations for your team

## Quick Reference

### Essential commands
```bash
claude mcp add <name> -- <command>          # Add server
claude mcp add <name> -s project -- <cmd>   # Add to project
claude mcp list                             # List servers
claude mcp remove <name>                    # Remove server
claude --mcp-debug                          # Debug mode
```

### Common connection strings
```bash
# Filesystem
npx -y @modelcontextprotocol/server-filesystem /path/to/dir

# GitHub  
npx -y @modelcontextprotocol/server-github

# Notion
npx -y @notionhq/notion-mcp-server

# Supabase
npx -y @supabase/mcp-server-supabase@latest --access-token TOKEN

# Memory
npx -y @modelcontextprotocol/server-memory
```

This guide provides everything needed to quickly connect existing MCP servers to Claude Code. Simply copy the relevant configuration, replace placeholder values with your actual credentials, and you're ready to go!