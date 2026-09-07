---
name: Track pages over time with the tab workspace
description: Keep a persistent workspace of audited URLs — open tabs, re-run audits, and read the history — using a guest token or a signed-in session.
api: openapi/pageaudit-openapi.json
operations: [create_guest, create_tab, list_tabs, run_tab, post_api_auth_start, post_api_auth_verify, post_api_auth_claim]
generated: '2026-09-07'
method: generated
---

# Track pages over time with the tab workspace

1. **Mint a guest token.** `POST /api/guest` (`create_guest`) returns `pa_…`. Send it as `X-Guest-Token`, `Authorization: Bearer pa_…` or `?guest_token=`. No account required; 20 tabs free per owner.
2. **Open a tab per URL.** `POST /api/tabs` (`create_tab`) with `{"url": "…", "alias": "…"}`. Replay-safe: calling it again for the same URL focuses the existing tab instead of duplicating it. Past 20 tabs the API answers 402 ($0.05 USDC per extra tab via x402).
3. **Re-audit on demand.** `POST /api/tabs/{id}/run` (`run_tab`) audits the tab's URL again and appends a new stored report — each run is a new immutable audit.
4. **Read the workspace.** `GET /api/tabs` (`list_tabs`) returns every tab with the active tab's latest result already rehydrated. Closing a tab (`DELETE /api/tabs/{id}`) keeps its audit history.
5. **Upgrade to an account when state matters.** `POST /api/auth/start` (`post_api_auth_start`) e-mails a 6-digit code; `POST /api/auth/verify` (`post_api_auth_verify`) exchanges it for a `sess_…` session and, on first confirmation, grants the 90-day full-access trial. `POST /api/auth/claim` (`post_api_auth_claim`) moves the guest's tabs and audits into the account so nothing is lost.
