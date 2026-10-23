---
title: "Virtualizor: The Login Parameter That Skips the Login"
url: "https://www.vulncheck.com/blog/virtualizor-billing-hook-unauthenticated-root-rce"
date: "2026-09-22"
author: "Valentin Lobstein (Chocapikk)"
feed_url: "https://www.vulncheck.com/feed/blog/atom.xml"
---
VulnCheck's Initial Access Intelligence team details an unauthenticated remote root RCE in the Virtualizor admin panel, where the pre-auth billing-module hook is guarded only against requests whose act is not 'login', so act=login walks straight into it, the hook unserializes an attacker POST field and splices its uid into a root shell, and the parameterized queries do not help because MySQL casts the injected string back to the integer the query expects.
