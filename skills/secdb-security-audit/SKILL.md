---
name: secdb-security-audit
description: Scan the current project for vulnerable application dependencies using ZEN SecDB purl_audit. Detects manifest files automatically (package.json, requirements.txt, Cargo.lock, go.mod, Gemfile.lock, pom.xml, composer.lock, *.csproj) and audits all dependencies. Use when the user asks about dependency vulnerabilities, security audit, or wants to check if the project has known CVEs.
metadata:
  author: Giuseppe Di Terlizzi
  version: 1.0.0
---

# Security Audit

Scan the current project for vulnerable dependencies using ZEN SecDB.

## Requirements

The **ZEN SecDB** MCP server must be configured. See README.md for setup instructions.

## Usage

```
/secdb-security-audit
```

## Steps

1. **Detect project type** by scanning for manifest files in the current directory and subdirectories:
   - Node.js: `package.json`, `package-lock.json`, `yarn.lock`
   - Python: `requirements.txt`, `Pipfile.lock`, `pyproject.toml`
   - Ruby: `Gemfile.lock`
   - Go: `go.mod`
   - Java: `pom.xml`, `build.gradle`, `build.gradle.kts`
   - Rust: `Cargo.lock`
   - PHP: `composer.lock`
   - .NET: `*.csproj`, `packages.lock.json`

2. **Extract dependencies** with their exact versions from the manifest files found.

3. **Convert to PURL format** following these rules:
   - npm: `pkg:npm/{name}@{version}`
   - pypi: `pkg:pypi/{name}@{version}` (lowercase name)
   - gem: `pkg:gem/{name}@{version}`
   - golang: `pkg:golang/{module}@{version}`
   - maven: `pkg:maven/{groupId}/{artifactId}@{version}`
   - cargo: `pkg:cargo/{name}@{version}`
   - composer: `pkg:composer/{vendor}/{name}@{version}`
   - nuget: `pkg:nuget/{name}@{version}`

4. **Call `purl_audit`** with the full PURL list. Only submit PURLs from supported ecosystems (npm, pypi, gem, golang, maven, cargo, composer, nuget).

5. **Present results**:
   - Never reproduce the full `report` JSON in the response
   - Show the `summary` directly
   - For each vulnerable package, report:
     - Advisory title and severity
     - Affected version range
     - Fixed version (if available in the advisory)
   - Prioritize Critical and High severity first
   - Suggest upgrade commands where applicable (e.g. `npm install`, `pip install --upgrade`)

## Going deeper

The `report` JSON contains the full advisory data and can be used for detailed analysis.
Additional ZEN SecDB MCP tools are available for deeper investigation on specific CVEs:

- **`vulnerability_info`** — full CVE details (description, references, affected versions)
- **`vulnerability_score`** — CVSS and current EPSS score
- **`epss_timeseries`** — historical EPSS trend for a CVE
- **`sightings_search`** — real-world exploitation sightings
- **`ssvc_calculator`** — CISA SSVC prioritization score

Use these tools when the user wants to investigate a specific CVE in depth, assess exploitability, or prioritize remediation.
