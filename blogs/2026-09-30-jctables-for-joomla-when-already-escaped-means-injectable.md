---
title: "JCTables for Joomla: When \"Already Escaped\" Means Injectable"
url: "https://www.vulncheck.com/blog/jctables-unauthenticated-sql-rw-to-rce"
date: "2026-09-30"
author: "Valentin Lobstein (Chocapikk)"
feed_url: "https://www.vulncheck.com/feed/blog/atom.xml"
---
VulnCheck's Initial Access Intelligence team details an unauthenticated JSON CRUD API in the JCTables component for Joomla whose escape helper trusts any value already wrapped in single quotes, turning one URL into arbitrary SQL read and write and, through a borrowed admin session, into remote code execution.
