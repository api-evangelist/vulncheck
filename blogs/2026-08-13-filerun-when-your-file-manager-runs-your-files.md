---
title: "FileRun: When Your File Manager Runs Your Files"
url: "https://www.vulncheck.com/blog/filerun-thumbnail-command-injection-rce"
date: "2026-08-13"
author: "Valentin Lobstein (Chocapikk)"
feed_url: "https://www.vulncheck.com/feed/blog/atom.xml"
---
VulnCheck's Initial Access Intelligence team details an OS command injection in FileRun's thumbnail generation, where the extractors pass user-controlled filenames to exec() without shell escaping, yielding remote code execution for any authenticated user or, through a public file-request weblink, without credentials.
