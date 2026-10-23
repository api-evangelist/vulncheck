---
title: "Monsta FTP: An SSRF Blocklist That Forgot IPv6 Exists"
url: "https://www.vulncheck.com/blog/monsta-ftp-ssrf-ipv6-blocklist-bypass"
date: "2026-07-14"
author: "Valentin Lobstein (Chocapikk)"
feed_url: "https://www.vulncheck.com/feed/blog/atom.xml"
---
VulnCheck's Initial Access Intelligence team details an unauthenticated SSRF in Monsta FTP 2.14.4, where an IPv4-only blocklist waves through the IPv4-mapped IPv6 form of the cloud metadata endpoint and bypasses both the front gate and the anti-rebinding recheck.
