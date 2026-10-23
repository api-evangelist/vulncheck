---
title: "SPIP odt2spip Plugin: Authenticated Command Injection via Uploaded Document Filename"
url: "https://www.vulncheck.com/blog/spip-odt2spip-authenticated-command-injection-rce"
date: "2026-10-16"
author: "Valentin Lobstein (Chocapikk)"
feed_url: "https://www.vulncheck.com/feed/blog/atom.xml"
---
VulnCheck's Initial Access Intelligence team details an authenticated OS command injection in the SPIP odt2spip plugin, where an uploaded document's filename is concatenated into a shell command and run, giving any low-privileged author remote code execution as the web server user.
