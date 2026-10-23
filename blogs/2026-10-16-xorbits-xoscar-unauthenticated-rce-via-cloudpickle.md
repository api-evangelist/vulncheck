---
title: "Xorbits Xoscar: Unauthenticated RCE via Cloudpickle Deserialization in the Actor Pool"
url: "https://www.vulncheck.com/blog/xorbits-xoscar-unauth-cloudpickle-rce"
date: "2026-10-16"
author: "Valentin Lobstein (Chocapikk)"
feed_url: "https://www.vulncheck.com/feed/blog/atom.xml"
---
VulnCheck's Initial Access Intelligence team details an unauthenticated remote code execution in Xoscar, the actor and RPC framework behind the Xinference inference stack, where the actor pool deserializes every inbound TCP message with cloudpickle before it checks the message type and with no authentication.
