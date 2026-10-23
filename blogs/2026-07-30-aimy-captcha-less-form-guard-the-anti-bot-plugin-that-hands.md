---
title: "Aimy Captcha-Less Form Guard: The Anti-Bot Plugin That Hands Bots the Keys"
url: "https://www.vulncheck.com/blog/aimy-captcha-less-form-guard-object-injection"
date: "2026-07-30"
author: "Valentin Lobstein (Chocapikk)"
feed_url: "https://www.vulncheck.com/feed/blog/atom.xml"
---
VulnCheck's Initial Access Intelligence team details an unauthenticated PHP object injection in the Aimy Captcha-Less Form Guard plugin for Joomla, where a repeating-key XOR published alongside its own ciphertext turns every protected form into an unserialize() sink and, on Joomla 3.9 through 5.2.1, into remote code execution.
