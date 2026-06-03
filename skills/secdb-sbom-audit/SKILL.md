---
name: secdb-sbom-audit
description: Audit a CycloneDX Software Bill of Materials (SBOM) file for known vulnerabilities using ZEN SecDB purl_audit. Use when the user provides or mentions a bom.json or bom.xml CycloneDX file and wants to check it for vulnerabilities.
metadata:
  author: Giuseppe Di Terlizzi
  version: 1.0.0
---

# SBOM Audit

Audit a CycloneDX Software Bill of Materials (SBOM) file for known vulnerabilities using ZEN SecDB.

## Requirements

The **ZEN SecDB** MCP server must be configured. See README.md for setup instructions.

## Usage

```
/secdb-sbom-audit [path/to/bom.json]
```

If no path is provided, scan the current directory for `bom.json` or `bom.xml` files.

## Steps

1. **Locate the SBOM file** — use the path provided, or search for `bom.json` / `bom.xml` in the project root.

2. **Parse the components** from the CycloneDX BOM:
   - For JSON: read `$.components[*]` entries
   - For XML: read `<components><component>` elements
   - Extract `purl` field from each component

3. **Filter PURLs** — only submit supported ecosystems to `purl_audit`:
   - Supported: `pkg:npm`, `pkg:pypi`, `pkg:gem`, `pkg:golang`, `pkg:maven`, `pkg:cargo`, `pkg:nuget`, `pkg:composer`
   - Skip unsupported types silently, report the count of skipped components at the end

4. **Call `purl_audit`** with the filtered PURL list.

5. **Present results**:
   - Never reproduce the full `report` JSON in the response
   - Show the audit summary
   - For each vulnerable component, cross-reference with the BOM to show:
     - Component name and version from the BOM
     - Advisory title and severity
     - Fixed version if available
   - Report skipped components count and ecosystems

## Going deeper

The `report` JSON contains the full advisory data and can be used for detailed analysis.
Additional ZEN SecDB MCP tools are available for deeper investigation on specific CVEs:

- **`vulnerability_info`** — full CVE details (description, references, affected versions)
- **`vulnerability_score`** — CVSS and current EPSS score
- **`epss_timeseries`** — historical EPSS trend for a CVE
- **`sightings_search`** — real-world exploitation sightings
- **`ssvc_calculator`** — CISA SSVC prioritization score

Use these tools when the user wants to investigate a specific CVE in depth, assess exploitability, or prioritize remediation.
