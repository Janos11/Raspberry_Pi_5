# Top 50 Safe-to-Block Domains from Pi-hole Logs

> This document explains **how to extract, classify, and maintain** a list of the *top safe-to-block domains* from your own Pi-hole logs.
>  
> It is designed to be **repeatable, educational, and safe**.  
> No guessing. No random internet lists.

---

## 1. Goal

- Identify **high-frequency domains** from your own network
- Block **ads, telemetry, and captive-portal checks**
- Avoid breaking:
  - Streaming services
  - Amazon / Google / Apple infrastructure
  - Core APIs

---

## 2. Data Source

Pi-hole stores query data in:

- `/etc/pihole/gravity.db` (SQLite database)

This is the **authoritative source** for domain frequency.

---

## 3. Extraction Command

Run this on your Pi-hole host:

```bash
sqlite3 /etc/pihole/gravity.db "
SELECT domain, COUNT(*) AS hits
FROM queries
WHERE status IN (2,3)
GROUP BY domain
ORDER BY hits DESC
LIMIT 100;
"
```

This produces the **top queried domains**.

---

## 4. Safety Filtering Rules

### Automatically SAFE to block
- Randomized subdomains
- Captive portal checks
- Analytics / telemetry endpoints

Examples:
- `*.captiveportal.*`
- `*.telemetry.*`
- `*.metrics.*`
- Long random prefixes

---

### NEVER block (exclude immediately)

Do **not** include domains matching:

- `api.amazon.com`
- `*.amazon.com` (root / regional)
- `*.google.com`
- `*.googlevideo.com`
- `*.icloud.com`
- `*.microsoft.com`

These are **infrastructure**, not ads.

---

## 5. Classification Categories

When reviewing domains, classify them into:

### A. Tracking / Analytics
- High-frequency
- Non-user-facing
- No functional dependency

### B. Captive Portal / Connectivity Checks
- Repeated probes
- Safe on home networks

### C. Streaming Telemetry (TEST FIRST)
- Prime Video / Netflix metrics
- Block cautiously

---

## 6. Example Output Format (Blocklist)

Create a file like `custom-blocklist.txt`:

```txt
# Captive portal checks
avsxappcaptiveportal.com

# Telemetry
metrics.example.net
telemetry.vendor.io

# Streaming telemetry (tested)
q419zmlyfb4fa0.zaz.api.amazonvideo.com
```

One domain per line.
No URLs.
No IPs.

---

## 7. GitHub Integration

1. Commit the blocklist file
2. Push to GitHub
3. Use the **RAW URL** in Pi-hole:
   - Group Management → Adlists
     ```txt
     https://raw.githubusercontent.com/Janos11/Raspberry_Pi_5/main/pihole/My_Ad_block_list.txt
     ```
4. Run **Update Gravity**

Your blocklist is now versioned and auditable.

---

## 8. Recommended Workflow

1. Review Pi-hole dashboard weekly
2. Extract top domains
3. Test block locally
4. Promote to GitHub list
5. Document reasoning

---

## 9. Philosophy

> Block **what your network actually uses**, not what strangers claim is bad.

This produces:
- Fewer breakages
- Higher block accuracy
- Better long-term maintainability

---

## 10. Next Enhancements

- Per-device blocking via Groups
- Separate lists: ads / telemetry / streaming
- Automation scripts
- Grafana dashboards

---

**Author:** Janos  
**Use case:** Home lab / Raspberry Pi / Pi-hole  
