---
name: Share a report and embed its score badge
description: Publish an audit under a non-enumerable slug, hand out the credential-free JSON or HTML report, embed the SVG score badge, and revoke the share when done.
api: openapi/pageaudit-openapi.json
operations: [post_api_audits_by_id_share, get_api_shared_by_slug, get_badge, delete_api_audits_by_id_share]
generated: '2026-09-07'
method: generated
---

# Share a report and embed its score badge

1. **Publish the audit.** `POST /api/audits/{id}/share` (`post_api_audits_by_id_share`) with your guest/session token publishes the report under a non-enumerable slug. Idempotent: calling again returns the same slug.
2. **Hand out the report.** Machines read `GET /api/shared/{slug}` (`get_api_shared_by_slug`) — full JSON, no credentials. Humans read `https://pageaudit.online/r/{slug}` (served with noindex).
3. **Embed the badge.** `GET /api/badge/{slug}` (`get_badge`) returns the badge metadata plus README-ready markdown; the image itself is `GET /badge/{slug}.svg`.
4. **Revoke when done.** `DELETE /api/audits/{id}/share` (`delete_api_audits_by_id_share`) revokes the share — the slug stops serving the report and the badge. The underlying audit keeps existing for its owner.
