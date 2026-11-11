<div align="center">

# Athar · أثر

**Network Forensics Field Manual & Analysis Workbench**

*The packets left a trace. Do the math on it.*

[![Live](https://img.shields.io/badge/LIVE-siteq8.github.io%2FAthar-3FE0D0?style=for-the-badge)](https://siteq8.github.io/Athar/)
![Offline](https://img.shields.io/badge/works-AIR--GAPPED-3FE0D0?style=for-the-badge)
![License](https://img.shields.io/badge/license-MIT-5B8DEF?style=for-the-badge)

A single-file, zero-dependency workbench for network forensics, threat hunting, and incident response. Drop it on an isolated analysis workstation with no internet and every local tool still runs.

</div>

---

## Why

Network forensics rarely fails because the analyst doesn't know *what* to look for. It fails because they're fighting BPF syntax, eyeballing beacon intervals, and squinting at a domain name wondering whether it's algorithmically generated. Athar does the arithmetic so you can spend your attention on the investigation.

Everything in tabs 02-05 is **pure client-side JavaScript**, no backend, no telemetry, no network calls. That's deliberate: analysis workstations are frequently air-gapped, and case evidence should never leave yours.

## Six tools

| # | Tool | What it does |
|---|------|--------------|
| **01** | **Command Forge** | Describe evidence in plain language → get the exact `tshark` / `zeek-cut` / `tcpdump` / `nfdump` / `editcap` / `Arkime` command. 12 curated templates covering DNS extraction, file carving, NetFlow, HTTP/TLS profiling, SMB lateral-movement hunting, evidence-grade capture, and huge-pcap splitting. |
| **02** | **BPF Builder** | Compose capture filters from dropdowns, direction, host/CIDR, protocol, ports, TCP flag presets, payload offsets, with a **plain-English translation** and drop-in usage for tcpdump/tshark/Wireshark. Stops you from silently dropping the evidence you needed. |
| **03** | **Beacon Hunter** | Paste Zeek `conn.log` (TSV or JSON) or any CSV. Groups by connection pair and scores periodicity from **median-absolute-deviation of inter-arrival times**, payload-size coefficient of variation, and observation count. Textbook C2 scores ~0.95; human browsing collapses. CSV export. |
| **04** | **DGA Scorer** | Rank domains on **Shannon entropy, English bigram plausibility, consonant-run length, digit ratio, label length, and TLD risk**. Cleanly separates dictionary-word domains from algorithmically generated ones (and long hex labels). CSV export. |
| **05** | **Field Reference** | Fast lookup: Zeek log cheat sheet, BPF primitives, tshark display filters, nfdump syntax, JA3/TLS notes, exfil & tunneling tells. Live filter box across every table. |
| **06** | **AI Analyst** | *Optional.* Bring your own Anthropic API key. Paste any artifact (Zeek line, TLS hello, NetFlow record, header block) and get triage / field-by-field explanation / pivot queries / a written case-brief finding. |

## Use it

**Hosted:** https://siteq8.github.io/Athar/

**Offline / air-gapped:**
```bash
git clone https://github.com/SiteQ8/Athar.git
# then just open Athar/index.html — no server, no build, no internet needed
```

## The statistics, briefly

- **Beacon score** = `0.55·jitter + 0.22·size-consistency + 0.23·confidence`, where *jitter* comes from `MAD(Δt)/median(Δt)` (to outliers, unlike stdev), *size-consistency* from the coefficient of variation of bytes, and *confidence* scales with `log(n)`. Pairs with fewer than 6 connections are flagged, you can't establish periodicity from four points.
- **DGA score** is a weighted blend dominated by **bigram implausibility** (each adjacent letter pair scored against English frequencies), because that's what best distinguishes `cloudflare` from `xkqzvhtmbrwlp`.

Both are **triage aids, not verdicts**, always corroborate beacons against jitter/protocol and domains against registration age, NXDOMAIN rate, and resolved infrastructure.

## Evidence-handling note

Tabs 02-05 never touch the network. Only the **AI Analyst** (tab 06) sends data out, directly from your browser to `api.anthropic.com` with your key. On live engagements that may conflict with chain-of-custody rules, so keep it to lab data and sanitized artifacts.

---

<div align="center">

Built by **[Ali AlEnezi](https://3li.info)** · [@SiteQ8](https://github.com/SiteQ8) · Kuwait 🇰🇼

**An independent open-source tool.** Not affiliated with or endorsed by any training provider, vendor, or certification body.

</div>
