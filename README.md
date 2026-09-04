# VulnCheck

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

VulnCheck is an exploit and vulnerability intelligence company. Its Exploit & Vulnerability
Intelligence platform enriches CVE records ahead of NIST NVD, publishes the VulnCheck KEV
catalog, and delivers exploit maturity, threat-actor, initial-access, IP and target
intelligence as machine-readable data.

- Website: https://www.vulncheck.com/
- Documentation: https://docs.vulncheck.com/
- API reference: https://docs.vulncheck.com/api
- Status: https://status.vulncheck.com/
- GitHub: https://github.com/vulncheck-oss

## API surface

| | |
|---|---|
| Base URL | `https://api.vulncheck.com/v3` |
| Contract | OpenAPI 3.1.0, fetched verbatim from `https://api.vulncheck.com/v3/openapi` (served anonymously) |
| Operations | 521 — 520 `GET` plus one bulk-lookup `POST /purls`. The API is read-only. |
| Schemas | 1,409 |
| Auth | VulnCheck API token as `Authorization: Bearer`, `?token=`, or a `token` cookie |
| Rate limit | 1,000 requests/minute on Community accounts; HTTP 429, no rate-limit headers |
| Events | None — no webhooks, no streaming, no AsyncAPI |

508 of the 521 operations are `GET /index/{name}`, one per named VulnCheck data feed.

## Agent surface

VulnCheck ships an unusually complete agent surface for a company its size:

- **MCP server** — `github.com/vulncheck-oss/mcp`, 24 tools. **Local stdio only**; there is no
  hosted endpoint. See `mcp/vulncheck-mcp.yml`.
- **Agent Skill** — VulnCheck publishes its own Claude Code skill at
  `github.com/vulncheck-oss/agent-tools`. Saved verbatim as `skills/vulncheck-cli.md`.
- **Agentic CLI contract** — from v1.0.0 the CLI documents a global `--json` flag, meaningful
  exit codes, a structured error envelope and a `vulncheck commands` capability dump.
- **llms.txt** — `https://docs.vulncheck.com/llms.txt`, 90KB, complete.

10 of the 24 MCP tools have no public REST equivalent — the whole v4 advisory family,
documentation search, component identification and the advisory digest. The MCP surface is
larger than the REST one, which is the reverse of the usual shape. See
`mcp/vulncheck-tool-crosswalk.yml`.

## Notable gaps

- **No `operationId` on any of the 521 operations.** Every generated SDK must synthesize method
  names from paths, and no artifact here can bind to a stable operation identifier.
- **No rate-limit response headers.** A client discovers the ceiling by hitting it.
- **No deprecation or sunset policy**, and no published SLA, for a product built on 490+ named
  indices that are added continuously.
- **No trust center and no named certification** (SOC 2, ISO 27001, FedRAMP) published anywhere
  on the estate.
- **No public pricing.** Only the free Community tier's terms are published.
- **Error bodies typed as bare `string`** on every operation, and 429 declared on none of them.

Each is recorded with evidence in the artifact directories, and the fixable spec ones are
captured as an OpenAPI Overlay in `overlays/`.
