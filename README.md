# ZEN SecDB Skills

Vulnerability scanning skills powered by [ZEN SecDB](https://secdb.nttzen.cloud).

Compatible with Claude Code, Codex CLI, Gemini CLI, GitHub Copilot, Cursor, and any agent that supports the [SKILL.md open standard](https://www.agensi.io/learn/agent-skills-open-standard).

## Install

**npx skills (recommended — works with all agents):**
```bash
npx skills add giterlizzi/secdb-skills
```

**Manual install**

Copy skills to your agent's directory:

| Agent | Directory |
|-------|-----------|
| Claude Code | `.claude/skills/` (project) or `~/.claude/skills/` (global) |
| Codex CLI | `.codex/skills/` or `~/.codex/skills/` |
| Gemini CLI | `.gemini/skills/` or `~/.gemini/skills/` |
| GitHub Copilot | `.github/skills/` |
| Cursor | `.cursor/skills/` |

```bash
git clone https://github.com/giterlizzi/secdb-skills

# Example: Claude Code global install
cp -r secdb-skills/skills/* ~/.claude/skills/
cp secdb-skills/CLAUDE.md ~/.claude/CLAUDE.md
```

## Skills

| Skill | Command | Description |
|-------|---------|-------------|
| `secdb-security-audit` | `/secdb-security-audit` | Scan all project dependencies for vulnerabilities |
| `secdb-sbom-audit` | `/secdb-sbom-audit [path]` | Audit a CycloneDX SBOM file |
| `secdb-audit-package` | `/secdb-audit-package [target]` | Check a specific package, PURL, or manifest file |

## Supported ecosystems

| Ecosystem     | PURL type        |
|---------------|------------------|
| Node.js       | `pkg:npm`        |
| Python        | `pkg:pypi`       |
| Ruby          | `pkg:gem`        |
| Go            | `pkg:golang`     |
| Java/JVM      | `pkg:maven`      |
| Rust          | `pkg:cargo`      |
| PHP           | `pkg:composer`   |
| .NET          | `pkg:nuget`      |

## MCP server

The skills require the ZEN SecDB MCP server.

**Automatic (Claude Code):** the `.mcp.json` in this repo configures the server automatically when you open the directory in Claude Code. No manual steps needed.

**Manual install:**

```bash
claude mcp add zen-secdb https://secdb.nttzen.cloud/mcp
```

Or add to `~/.claude/mcp.json` (global) or `.mcp.json` (project):

```json
{
  "mcpServers": {
    "zen-secdb": {
      "url": "https://secdb.nttzen.cloud/mcp"
    }
  }
}
```

→ [MCP Documentation](https://secdb.nttzen.cloud/docs/integrations/mcp)

## Additional info

- [ZEN SecDB](https://secdb.nttzen.cloud) - Vulnerability intelligence platform
- ZEN SecDB MCP Server — endpoint: `https://secdb.nttzen.cloud/mcp` · [docs](https://secdb.nttzen.cloud/docs/integrations/mcp)

## License

MIT
