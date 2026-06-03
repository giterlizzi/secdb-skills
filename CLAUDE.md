# Claude Code — ZEN SecDB Integration

## Security Scanning Tools

ZEN SecDB MCP server is available for vulnerability scanning. Use it proactively when working on projects with dependencies.

### Available tools

- **`purl_audit`** — audit application dependencies by PURL (npm, pypi, gem, golang, maven, cargo, composer, nuget)

### Additional tools for CVE deep-dive

Once vulnerabilities are found, use these tools to investigate specific CVEs:

- **`vulnerability_info`** — full CVE details (description, references, affected versions)
- **`vulnerability_score`** — CVSS and current EPSS score
- **`epss_timeseries`** — historical EPSS trend for a CVE
- **`sightings_search`** — real-world exploitation sightings
- **`ssvc_calculator`** — CISA SSVC prioritization score

### When to use `purl_audit`

- User asks about dependency vulnerabilities
- User asks if a specific package version is safe
- User adds or upgrades a dependency
- User opens a project with manifest files (suggest running `/project:secdb-security-audit`)
- User provides a CycloneDX SBOM file

### PURL format quick reference

| Ecosystem     | Format                                        | Example |
|---------------|-----------------------------------------------|---------|
| npm           | `pkg:npm/{name}@{version}`                    | `pkg:npm/lodash@4.17.21` |
| pypi          | `pkg:pypi/{name}@{version}`                   | `pkg:pypi/django@4.2.0` |
| gem           | `pkg:gem/{name}@{version}`                    | `pkg:gem/rails@7.0.0` |
| golang        | `pkg:golang/{module}@{version}`               | `pkg:golang/github.com/gin-gonic/gin@1.9.1` |
| maven         | `pkg:maven/{groupId}/{artifactId}@{version}`  | `pkg:maven/org.apache.logging.log4j/log4j-core@2.17.1` |
| cargo         | `pkg:cargo/{name}@{version}`                  | `pkg:cargo/openssl-src@111.10` |
| composer      | `pkg:composer/{vendor}/{name}@{version}`      | `pkg:composer/symfony/symfony@6.4.0` |
| nuget         | `pkg:nuget/{name}@{version}`                  | `pkg:nuget/Newtonsoft.Json@13.0.1` |

### Guidelines

- Never guess whether a package is vulnerable — always call `purl_audit`
- Never reproduce the full `report` JSON in the response — use `summary` and extract key fields only
- Always use exact versions from lock files, not ranges from manifest files
- Prioritize Critical and High severity findings in the response
- Suggest fixed versions from advisory data when available
