---
name: vulncheck-c2-blocklist
description: >-
  Pull VulnCheck command-and-control infrastructure feeds and initial-access detection rules to
  feed a firewall, protective DNS resolver, or IDS. Invoke when asked to build a blocklist, get
  C2 indicators, feed protective DNS, or obtain Suricata/Snort rules for recent exploitation.
api: VulnCheck API v3
base_url: https://api.vulncheck.com/v3
generated: '2026-09-04'
method: generated
source: openapi/vulncheck-api-openapi.json + https://docs.vulncheck.com/integrations
operations:
- GET /pdns/vulncheck-c2
- GET /tags/vulncheck-c2
- GET /rules/initial-access/{type}
- GET /index/ipintel-3d
- GET /index/ipintel-10d
- GET /index/ipintel-30d
- GET /index/ipintel-90d
- GET /index/botnets
---

# Build blocklists and detections from VulnCheck

Operations are named by **method + path** — the spec declares no `operationId`.

## The three feeds

### C2 hostnames, for protective DNS

`GET /pdns/vulncheck-c2?format=<format>`

Takes a `format` parameter so the output can be consumed directly by a protective-DNS resolver
rather than reshaped. VulnCheck documents this feed as the input to Checkpoint Quantum, Cisco
ASA, Fortinet FortiGate and Palo Alto NGFW deployments.

### C2 IP addresses, for firewall denylists

`GET /tags/vulncheck-c2?format=<format>`

The path says "tags" but the payload is IP addresses. Same `format` parameter.

### Detection rules, for an IDS

`GET /rules/initial-access/suricata`
`GET /rules/initial-access/snort`

`{type}` accepts only `suricata` and `snort`. These are the rules for VulnCheck's Initial
Access Intelligence — exploitation VulnCheck has observed being used to gain a foothold.

## Adding IP intelligence with a time window

`GET /index/ipintel-3d`, `/index/ipintel-10d`, `/index/ipintel-30d`, `/index/ipintel-90d`

Four separate indices, one per rolling window. Pick the window deliberately: 3d is the tightest
and least noisy, 90d is the broadest and will include infrastructure that has since gone quiet.

These records carry geolocation and ASN enrichment, and cover attacker infrastructure,
honeypots, and hosts observed being targeted by initial-access exploits — three different
things. Read the classification before blocking anything; a targeted host is a victim, not an
attacker, and blocking it is the wrong action.

## Operational notes

- **Read-only.** Nothing here changes VulnCheck state. What it changes is *your* firewall, and
  that reversal is on your side, not the API's.
- **Poll; there are no webhooks.** VulnCheck publishes no event, streaming or webhook surface.
  Refresh on a schedule that matches the feed's window, and use the cursor
  (`start_cursor=true`, then `cursor=<next_cursor>`) for the index endpoints.
- **1,000 requests per minute** on Community accounts, with no header telling you how much
  budget is left. A scheduled full-feed pull should be paced, not burst.
- For bulk or air-gapped use, prefer the offline route: `GET /backup` then
  `GET /backup/{index}` returns pre-signed archive links, and the VulnCheck CLI wraps this as
  `vulncheck offline sync`.
