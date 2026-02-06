---
title: Systemarchitektur Übersicht
---

Kurz: Übersicht über Komponenten, Datenfluss und Deployment‑Sketch für MVP (Frontend: SvelteKit, Backend: FastAPI, DB: Postgres+PostGIS).

Komponenten

- Frontend: SvelteKit + MapLibre (Map UI)
- Backend: FastAPI REST API, Auth, Moderation Services
- Persistenz: PostgreSQL + PostGIS, Objektspeicher für Attachments
- Infrastruktur: Reverse Proxy (NGINX), optional CDN für Assets

Datenfluss (vereinfachte Sequenz)

1. Client lädt Map (Vector Tiles extern) und sichtbare Markings via `GET /markings?bbox=...`.
2. Nutzer erstellt Marking → `POST /markings` (optimistic UI); Server validiert, speichert, ggf. markiert `pending_review`.
3. Moderation: Reports erzeugen `reports` Einträge; Moderatoren bearbeiten via Admin UI.

Deployment‑Hinweise

- Staging und Production getrennt; DB‑Backups regelmäßig; Indizes `CONCURRENTLY` auf großen Tabellen erstellen.
- Monitoring: Health endpoint `/health`, basic metrics (requests, errors, moderation queue length).

Skalierung & Performance

- Read‑Optimized: Cache/Redis für häufige bbox‑queries; Vector tiles extern.
- Writes: Moderation pipeline asynch, background workers for heavy tasks (e.g., image processing).

Quellen: ChatGPT_Architecture_Discussion.md, ChatGPT_SPEC_doc.md, ChatGPT_MVP_Struktur.md
