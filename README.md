<div align="center">

# 🛰️ Athar · أثر

**Network Forensics Field Manual & Analysis Workbench**

*The packets left a trace. Do the math on it.*

[![Live](https://img.shields.io/badge/LIVE-siteq8.github.io%2FAthar-3FE0D0?style=for-the-badge)](https://siteq8.github.io/Athar/)
![Offline](https://img.shields.io/badge/works-AIR--GAPPED-3FE0D0?style=for-the-badge)
![License](https://img.shields.io/badge/license-MIT-5B8DEF?style=for-the-badge)

A single-file, zero-dependency companion for **SANS FOR572** (Advanced Network Forensics) and any network threat-hunting work. Drop it on a lab VM with no internet and every local tool still runs.

</div>

---

## Why

By day three of FOR572 you know *what* to look for — but you're fighting BPF syntax, eyeballing beacon intervals, and squinting at domain names wondering if they're a DGA. Athar does the arithmetic so you can spend attention on the investigation.

Everything in tabs 02–05 is **pure client-side JavaScript** — no backend, no telemetry, no network calls. That's deliberate: the FOR572 VM environment is offline, and case evidence shouldn't leave your workstation.

## Six tools, mapped to how the course is taught

| # | Tool | What it does |
|---|------|--------------|
| **01** | **Command Forge** | Describe evidence in plain language → get the exact `tshark` / `zeek-cut` / `tcpdump` / `nfdump` / `editcap` / `Arkime` command. 12 curated templates covering DNS extraction, file carving, NetFlow, HTTP/TLS profiling, SMB lateral-movement hunting, evidence-grade capture, and huge-pcap splitting. |
| **02** | **BPF Builder** | Compose capture filters from dropdowns — direction, host/CIDR, protocol, ports, TCP flag presets, payload offsets — with a **plain-English translation** and drop-in usage for tcpdump/tshark/Wireshark. Stops you from silently dropping the evidence you needed. |
| **03** | **Beacon Hunter** | Paste Zeek `conn.log` (TSV or JSON) or any CSV. Groups by connection pair and scores periodicity from **median-absolute-deviation of inter-arrival times**, payload-size coefficient of variation, and observation count. Textbook C2 scores ~0.95; human browsing collapses. CSV export. |
| **04** | **DGA Scorer** | Rank domains on **Shannon entropy, English bigram plausibility, consonant-run length, digit ratio, label length, and TLD risk**. Cleanly separates dictionary-word domains from algorithmically generated ones (and long hex labels). CSV export. |
| **05** | **Field Reference** | Lectern-speed lookup — Zeek log cheat sheet, BPF primitives, tshark display filters, nfdump syntax, JA3/TLS notes, exfil & tunneling tells. Live filter box. No scrolling a PDF at 14:40. |
| **06** | **AI Analyst** | *Optional.* Bring your own Anthropic API key. Paste any artifact (Zeek line, TLS hello, NetFlow record, header block) and get triage / field-by-field explanation / pivot queries / capstone-brief finding. |

## Use it

**Hosted:** https://siteq8.github.io/Athar/

**Offline / in the lab VM:**
```bash
git clone https://github.com/SiteQ8/Athar.git
# then just open Athar/index.html — no server, no build, no internet needed
```

## The statistics, briefly

- **Beacon score** = `0.55·jitter + 0.22·size-consistency + 0.23·confidence`, where *jitter* comes from `MAD(Δt)/median(Δt)` (robust to outliers, unlike stdev), *size-consistency* from the coefficient of variation of bytes, and *confidence* scales with `log(n)`. Pairs with fewer than 6 connections are flagged — you can't establish periodicity from four points.
- **DGA score** is a weighted blend dominated by **bigram implausibility** (each adjacent letter pair scored against English frequencies), because that's what best distinguishes `cloudflare` from `xkqzvhtmbrwlp`.

Both are **triage aids, not verdicts** — always corroborate beacons against jitter/protocol and domains against registration age, NXDOMAIN rate, and resolved infrastructure.

## Evidence-handling note

Tabs 02–05 never touch the network. Only the **AI Analyst** (tab 06) sends data out — directly from your browser to `api.anthropic.com` with your key. On live engagements that may conflict with chain-of-custody rules, so keep it to lab data and sanitized artifacts.

---

<div align="center">

Built by **[Ali AlEnezi](https://3li.info)** · [@SiteQ8](https://github.com/SiteQ8) · Kuwait 🇰🇼

Companion to SANS FOR572 by Phil Hagen. **Not affiliated with, endorsed by, or produced by SANS Institute or GIAC.**

</div>
