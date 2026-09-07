---
name: Audit a page and apply the ready-made patch
description: Run a one-shot technical SEO audit of a URL, read every finding with its fix, and fetch the consolidated patch (head block, robots, sitemap) ready to apply.
api: openapi/pageaudit-openapi.json
operations: [audit_url, create_guest, get_audit, get_patch]
generated: '2026-09-07'
method: generated
---

# Audit a page and apply the ready-made patch

1. **Audit in one call, no token needed.** `POST /api/audit` (`audit_url`) with `{"url": "https://example.com/"}`. The 200 body carries `summary`, `issues[]` (each finding), `fixes[]` (the ready fix per finding) and `quota`. Free tier: 10 audits per IP per day, 300/hour ceiling.
2. **Keep the id.** To re-read later without re-auditing, you need a credential: mint a guest token first with `POST /api/guest` (`create_guest`) and send it on the audit call as `X-Guest-Token: pa_…` (or `Authorization: Bearer pa_…`).
3. **Re-read the audit.** `GET /api/audits/{id}` (`get_audit`) returns the stored report in full, including `fixes[]`. Audits are immutable rows — a new state of the page needs a new run.
4. **Fetch the patch.** `GET /api/audits/{id}/patch` (`get_patch`) returns the consolidated fix: the `<head>` block to paste, the files to create at the site root (robots.txt, sitemap), and templates for what only the owner can fill in. No language model is involved — only facts from the page itself.
5. **Handle the gate honestly.** A 402 means the daily allowance is spent: the body's `accepts[]` describes the x402 payment ($0.02 USDC on Base) — pay and repeat the same call with `X-PAYMENT`. Alternatives: a human solves Turnstile; or sign up and confirm the e-mail for a 90-day free trial. A 429 means the hourly ceiling — wait, do not pay. Check `GET /api/gate` before bulk work.
