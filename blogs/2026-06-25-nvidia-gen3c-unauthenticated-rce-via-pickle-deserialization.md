---
title: "NVIDIA GEN3C: Unauthenticated RCE via Pickle Deserialization in the Inference API"
url: "https://www.vulncheck.com/blog/nvidia-gen3c-unauth-pickle-rce"
date: "2026-06-25"
author: "Valentin Lobstein (Chocapikk)"
feed_url: "https://www.vulncheck.com/feed/blog/atom.xml"
---
VulnCheck's Initial Access Intelligence team details an unauthenticated remote code execution in NVIDIA's GEN3C, where two FastAPI inference endpoints deserialize raw HTTP request bodies with pickle.loads() with no authentication.
