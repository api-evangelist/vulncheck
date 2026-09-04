---
name: vulncheck-cli
description: Use the VulnCheck CLI to query vulnerability and exploit intelligence. Invoke when the user asks about listing or querying VulnCheck indices, security advisories, vendor advisories, CVE lookups, downloading index backups, known exploited vulnerabilities, KEV, CISA KEV, NVD, package security, PURL lookups (npm, pypi, maven, cargo, golang, nuget), CPE lookups, scanning a project or directory for vulnerable dependencies, exploits, Initial Access Intelligence, detection rules (Snort, Suricata), IP intelligence, C2 infrastructure, botnet tracking, protective DNS, or offline vulnerability scanning.
compatibility: Requires the `vulncheck` CLI binary, version 1.0.0 or later, installed and authenticated. On Windows also requires WSL or Git Bash.
---

# VulnCheck CLI <!-- skill-version: 1.0.0 -->

> **Binary is `vulncheck`** — not `vulncheck-cli`, not `vci`. Run commands directly, no `||` fallbacks or `echo` error strings. VulnCheck is a standalone CLI — it does not require a project directory.

Only authenticate if a command fails with exit `3` or `.error.code == "auth_required"` — run `vulncheck auth login` or set `VC_TOKEN`.

## The agentic contract

The CLI ships a stable wire contract designed for agents. Rely on it:

- **`--json` is a global flag.** Every command accepts it. Stdout carries only the JSON payload; info, progress, spinners, and warnings all route to stderr. `vulncheck <cmd> --json | jq` always works.
- **Exit codes are meaningful.** `0` success · `1` internal · `2` validation · `3` auth · `4` not_found · `5` rate_limited · `6` network · `130` cancelled (SIGINT). Dispatch on the code, not the message text.
- **Errors are structured.** In `--json` mode, failures emit `{"schema_version":1,"error":{"code":"...","message":"...","http_status":...}}` on stdout. Match on `.error.code` (one of `auth_required`, `auth_invalid`, `validation`, `not_found`, `rate_limited`, `network`, `bad_request`, `cancelled`, `internal`).
- **`--no-interactive` (or non-TTY / `CI=1`) refuses to block on prompts.** Interactive commands (`auth login`) fail fast with exit `2`; some degrade gracefully (see command-specific notes below).
- **`vulncheck commands` dumps the whole command tree as JSON** — probe capabilities without parsing `--help`.
- **`schema_version: 1`** appears on every CLI-shaped payload. Bump-guard your consumer against future breaks.
- **Execute directly.** When intent is clear, run the command — never present a list of variants to choose from. Do only what was asked; return the raw output and stop. Never add filtering, sorting, or summarisation that wasn't requested.
- **Always use `--json`** unless the user explicitly requests plain text or tabular output.
- **On pipeline failure,** drop the pipe and read raw output directly — the CLI always emits valid JSON on stdout; don't iterate on the pipeline.

Diagnostics / capability probing (not required before every task):

```bash
vulncheck auth status --json | jq -e '.authenticated'  # exit 0 iff authenticated
vulncheck version --json | jq -r .version              # probe CLI version
vulncheck commands | jq '.root.subcommands[].name'     # discover commands
```

## Auth

```bash
vulncheck auth status --json    # {schema_version, authenticated, token_source, user, email}
                                # exit 0 regardless — dispatch on .authenticated
vulncheck auth login            # interactive; refuses under --no-interactive
vulncheck auth logout
```

## Indices

VulnCheck has 500+ named indices — vendor advisories, CVE feeds, exploit data, KEV lists, and more.

**List all available indices** (only when discovering names — if you already know the index, query it directly):

```bash
vulncheck indices list --json                      # [{name, description, href}, ...]
vulncheck indices list --json | jq '.[].name'
vulncheck indices list --json | jq '.[] | select(.name | contains("nvd"))'
vulncheck indices browse                           # interactive fuzzy picker; degrades to JSON list under non-TTY
```

**Query an index** (JSON on stdout, one array of data records):

```bash
vulncheck index list <index-name> --json
vulncheck index list <index-name> --json --cve CVE-2021-44228
vulncheck index list <index-name> --json --limit 10
vulncheck index list <index-name> --json --sort date_added
vulncheck index list <index-name> --json --pubStartDate 2024-01-01 --pubEndDate 2024-06-30
vulncheck index list <index-name> --json --cursor <value>    # next page
vulncheck index list <index-name> --json --all               # auto-paginate every page into one array
vulncheck index browse <index-name>                          # interactive document browser; non-TTY behavior untested
```

`--all` walks `next_cursor` end-to-end — use for full-index dumps. For 30+ additional filter flags (CIDR, ASN, threat actor, ransomware, MITRE ID, etc.) run `vulncheck index list --help` or dump the surface via `vulncheck commands | jq '.root.subcommands[] | select(.name=="index")'`.

**Common indices:**
| Index | Contents |
|-------|----------|
| `vulncheck-nvd2` | NVD CVE data enriched by VulnCheck |
| `vulncheck-kev` | VulnCheck Known Exploited Vulnerabilities |
| `cisa-kev` | CISA KEV catalog |
| `initial-access` | Initial Access Intelligence (exploits, PoCs) |
| `ipintel-3d` / `ipintel-10d` / `ipintel-30d` / `ipintel-90d` | IP Intelligence (by timeframe) |

**Download a backup:**

Two paths — both work headless:

```bash
# Streamed download with atomic .part rename, progress on stderr
vulncheck backup download <index-name> --no-interactive --json

# Print the signed URL
vulncheck backup url <index-name> --json | jq -r '.url'

# Download via curl
vulncheck backup url <index-name> --json | jq -r '.url' | xargs curl -OL
```

## Utility

```bash
vulncheck version --json                      # {schema_version, version, build_date, changelog_url}
vulncheck upgrade status                      # check for a newer release
vulncheck upgrade latest                      # install the newest release
vulncheck commands                            # full JSON command tree (schema_version, root, subcommands, flags)
vulncheck commands | jq '.root.subcommands[] | select(.name=="scan") | .flags'  # discover scan's flags
```

## Reference Files

| Task                                                         | Reference                                                      |
| ------------------------------------------------------------ | -------------------------------------------------------------- |
| PURL and CPE package lookups (single + batch)                | [references/purl-cpe.md](references/purl-cpe.md)               |
| Scan a project directory for vulnerable dependencies         | [references/scanning.md](references/scanning.md)               |
| Air-gapped / offline scanning and lookups                    | [references/offline.md](references/offline.md)                 |
| IP intelligence, C2 infrastructure, protective DNS           | [references/ip-intelligence.md](references/ip-intelligence.md) |
| Detection rules, Snort/Suricata, Initial Access Intelligence | [references/initial-access.md](references/initial-access.md)   |
| API token management (safe secret handling)                  | [references/token.md](references/token.md)                     |
