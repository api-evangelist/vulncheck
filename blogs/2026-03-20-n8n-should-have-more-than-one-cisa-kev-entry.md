---
title: "n8n Should Have More Than One CISA KEV Entry"
url: "https://www.vulncheck.com/blog/n8n-needs-more-kev"
date: "2026-03-20"
author: "Jacob Baines"
feed_url: "https://www.vulncheck.com/feed/blog/atom.xml"
---
CISA recently added CVE-2025-68613, an authenticated n8n RCE, to its KEV catalog, but that framing misses how the vulnerability is actually exploited. In practice, it can be paired with CVE-2026-21858, an authentication bypass that is already being actively exploited in the wild. With over 14,000 exposed n8n instances, this attack path is both practical and widespread.
