---
title: Konsolidierte SPEC (Projekt‑Referenz)
---

Kurz: Dieses Dokument fasst Anforderungen, Must‑Have‑Funktionen und Meilensteine zusammen. Es dient als zentrale Referenz; Detail‑Specs befinden sich in `docs/architecture/api.md` und `docs/architecture/database-schema.md`.

1. Hintergrund & Ziel

- Ziel: Low‑friction Karte für öffentliches Feedback; Fokus auf einfache Einreichung, Moderation und Datenschutz.

2. Must‑Have (Alpha)

- Sichtbare Markings (GET /markings)
- Einreichen von Markings (POST /markings) — anonym möglich
- Moderation: Reports + basic admin UI
- Datenschutz: anonyme Abgabe, Retention Policy (90 Tage)

3. Should‑Have (Beta)

- Comments & Voting (realtime counts)
- Snapping Verbesserungen; better UX für mobile
- Export/Integration (GeoJSON/CSV) für Verwaltungen

4. Nicht‑Ziele (zurzeit)

- Self‑hosted Vector‑Tile‑Stack (zu hohe Kosten); externe Provider nutzen

5. Meilensteine / Roadmap (Kurz)

- Phase 0 — Prototyp: Map UI, Pin submission, basic backend
- Alpha — Basic public launch: API, Moderation, Privacy docs
- Beta — Feedback loops, Citybuilder opt‑in, Integrations

6. Links / Referenzen

- API Spec: `docs/architecture/api.md`
- DB Schema: `docs/architecture/database-schema.md`
- Moderation: `docs/concepts/moderation.md`
- Privacy: `docs/concepts/privacy.md`
