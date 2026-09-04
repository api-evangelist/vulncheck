---
name: vulncheck-dependency-vulnerabilities
description: >-
  Check software dependencies and installed products for known vulnerabilities using VulnCheck,
  by Package URL (PURL) for open-source packages or by CPE 2.3 for vendor products. Invoke when
  asked to check a package, an SBOM, a dependency list, or an installed product version for CVEs.
api: VulnCheck API v3
base_url: https://api.vulncheck.com/v3
generated: '2026-09-04'
method: generated
source: openapi/vulncheck-api-openapi.json + data-model/vulncheck-data-model.yml
operations:
- GET /purl
- POST /purls
- GET /cpe
- GET /search/cpe
- GET /index/cpe-vulnerable
---

# Check dependencies for vulnerabilities with VulnCheck

Operations are named by **method + path**; the VulnCheck spec declares no `operationId`.

Authenticate every call with `Authorization: Bearer <token>`.

## Choose the right identifier

- **PURL** for anything from a package ecosystem — npm, PyPI, Maven, Cargo, Go, NuGet, Composer,
  opam, Alpine, Debian, Ubuntu, Rocky. Format: `pkg:type/namespace/name@version`.
- **CPE 2.3** for vendor products — appliances, operating systems, commercial software.

Getting this wrong is the usual cause of a false negative: package-ecosystem advisories
frequently carry no CPE at all, so a CPE query will simply miss them.

## Steps

### One package

`GET /purl?purl=pkg:npm/lodash@4.17.20`

Returns the parsed PURL (`purl_struct`: type, namespace, name, version, qualifiers, subpath)
alongside the associated `cves` array.

### A whole dependency list or SBOM

`POST /purls`

Send the list of PURLs in the request body. This is the only POST in the entire API and it is
still a **lookup** — it creates nothing and changes nothing. Use it instead of looping over
`GET /purl`, both for speed and to stay inside the 1,000 requests-per-minute ceiling.

Each result is a `purl.BatchVulnFinding`: `purl`, `purl_struct`, `cves`.

### A vendor product you know the CPE for

`GET /cpe?cpe=cpe:2.3:a:vendor:product:1.2.3:*:*:*:*:*:*:*`

Add `isVulnerable=true` to restrict to CVEs where that CPE is confirmed vulnerable rather than
merely referenced.

### A vendor product you do not have the CPE for

`GET /search/cpe?vendor=<vendor>&product=<product>&version=<version>&part=a`

Returns matching CPEs and their CVEs. Be specific: a broad vendor-only query returns every CPE
that vendor has ever registered, and those responses reach tens of megabytes. There is no
field-selection or sparse-fieldset parameter on this API, so you cannot ask for less.

## After you have the CVE list

Hand each CVE to the `vulncheck-cve-triage` skill to establish exploitation status. A CVE list
alone is a backlog; a CVE list annotated with KEV membership and initial-access activity is a
priority order.

## Errors

Same envelope throughout: `{"error": true, "errors": ["..."]}` — not RFC 9457 problem+json.
`401` means the token is missing or expired (30 days idle); `429` means back off, with no
`Retry-After` to guide you.
