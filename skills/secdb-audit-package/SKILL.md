---
name: secdb-audit-package
description: Check a specific package, PURL, or manifest file for known vulnerabilities using ZEN SecDB purl_audit. Use when the user asks about a specific package version, provides a PURL string, or wants to check a single dependency file.
metadata:
  author: Giuseppe Di Terlizzi
  version: 1.0.0
---

# Audit Package

Check a specific package or dependency file for known vulnerabilities using ZEN SecDB.

## Requirements

The **ZEN SecDB** MCP server must be configured. See README.md for setup instructions.

## Usage

```
/project:audit-package [target]
```

Where `[target]` can be:
- A PURL string: `pkg:npm/lodash@4.17.20`
- A package name and version: `lodash 4.17.20` or `django==3.2.0`
- A specific manifest file: `path/to/requirements.txt`

## Steps

1. **Identify the input type**:
   - If it looks like a PURL (`pkg:...`), use it directly
   - If it's a `name version` or `name==version` string, convert to PURL — ask the user for the ecosystem if ambiguous
   - If it's a file path, parse it as a manifest and extract all dependencies

2. **Convert to PURL** if needed, following ecosystem conventions:
   - npm: `pkg:npm/{name}@{version}`
   - pypi: `pkg:pypi/{name}@{version}` (lowercase)
   - gem: `pkg:gem/{name}@{version}`
   - golang: `pkg:golang/{module}@{version}`
   - maven: `pkg:maven/{groupId}/{artifactId}@{version}`
   - cargo: `pkg:cargo/{name}@{version}`
   - composer: `pkg:composer/{vendor}/{name}@{version}`
   - nuget: `pkg:nuget/{name}@{version}`

3. **Call `purl_audit`** with the PURL(s).

4. **Present results** with full advisory details:
   - Never reproduce the full `report` JSON in the response
   - Severity and CVSS score
   - CVE identifiers
   - Description summary
   - Affected version range
   - Fixed version and upgrade instructions

## Going deeper

The `report` JSON contains the full advisory data and can be used for detailed analysis.
Additional ZEN SecDB MCP tools are available for deeper investigation on specific CVEs:

- **`vulnerability_info`** — full CVE details (description, references, affected versions)
- **`vulnerability_score`** — CVSS and current EPSS score
- **`epss_timeseries`** — historical EPSS trend for a CVE
- **`sightings_search`** — real-world exploitation sightings
- **`ssvc_calculator`** — CISA SSVC prioritization score

Use these tools when the user wants to investigate a specific CVE in depth, assess exploitability, or prioritize remediation.
