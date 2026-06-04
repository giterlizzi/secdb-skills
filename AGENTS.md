# AGENTS.md

## Overview

This repository contains vulnerability scanning skills for AI coding agents,
powered by [ZEN SecDB](https://secdb.nttzen.cloud).

## Available skills

| Skill | Command | When to use |
|-------|---------|-------------|
| `secdb-security-audit` | `/secdb-security-audit` | Scan all project dependencies for vulnerabilities — run before every PR |
| `secdb-sbom-audit` | `/secdb-sbom-audit [path]` | Audit a CycloneDX SBOM file — use when a `bom.json` or `bom.xml` is present |
| `secdb-audit-package` | `/secdb-audit-package [target]` | Check a specific package, PURL, or manifest file — use when adding or upgrading a single dependency |

## Security audit (SCA)

This repository uses its own skills to audit dependencies.
When contributing or reviewing changes, run a security audit:

1. Ensure the ZEN SecDB MCP server is configured.

Eg. for Claude
```bash
claude mcp add zen-secdb https://secdb.nttzen.cloud/mcp
```

2. Run the security audit skill:
```
/secdb-security-audit
```

3. Fix any Critical or High severity findings before opening a PR.

## Skill structure

Each skill lives in `skills/{skill-name}/SKILL.md` with YAML frontmatter:

```yaml
---
name: skill-name
description: Clear description with activation triggers
---
```

## Security considerations

- Never commit credentials or API keys
- The ZEN SecDB MCP endpoint is public — no authentication required
- Run `/secdb-security-audit` on any project before merging dependency changes
