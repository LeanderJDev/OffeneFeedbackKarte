---
title: 2026-02-06 — AI‑unterstützte Dokumentationsarbeit
---

Kurz: Protokoll der AI‑gestützten Überführung von internen `ChatGPT_*`-Notizen in die Projekt‑Docs.

Was wurde gemacht

- Aufgeteilt: `privacy-and-moderation.md` → `docs/concepts/privacy.md` und `docs/concepts/moderation.md`.
- Retention‑Entscheidung für Moderationsdaten dokumentiert: **90 Tage** (`docs/decisions/retention-moderation.md`).
- Mapstack‑Entscheidung: nicht self‑hosted; Dokumentation in `docs/architecture/mapstack.md`.
- API‑Spec (erste Version) erstellt: `docs/architecture/api.md`.
- Datenbankschema dokumentiert: `docs/architecture/database-schema.md`.
- Mehrere Concept/Research‑Dateien aus `docs/_notes/` übernommen:
    - `naming-considerations.md`, `user-participation.md`, `ui-principles.md`, `map-interaction.md`
    - `research/*`: existing‑platforms, osm‑limitations, citybuilder‑ui‑evaluation, comparable‑projects
    - Architektur‑Overview: `docs/architecture/overview.md`

Quellen / Referenzen

- Alle Inhalte stammen aus internen Notizen (`docs/_notes/ChatGPT_*.md`). Änderungen sind redaktionell strukturiert, inhaltliche Entscheidungen (z. B. Lizenz) bleiben als ADR‑Platzhalter.

Entscheidungen & Begründungen

- Moderations‑Retention = 90 Tage: ausreichend für Early‑Phase‑Investigations, begrenzt datenschutzrechtliche Risiken.
- Mapstack extern: reduziert initialen Betriebsaufwand und Kosten.
- Lizenzfrage: ADR vorgesehen, wird separat ausgefüllt.

Next steps

1. `docs/_notes/Dokumentationsdateien.md` auf Konsistenz prüfen (Index stimmt bereits mit erstellten Dateien überein).
2. Optional: Feinschliff der API‑Spec (Error‑Schemas, Auth‑Flows). Priorität: hoch.
3. Ergänzung: Konkrete Moderations‑UI‑Aufgaben und Threshold‑Tests.

Agenten‑Metadaten

- Aktionen durchgeführt: Datei‑Erstellungen (Docs), Todo‑Tracker aktualisiert.
- Werkzeuge: lokale Repo‑Bearbeitung, strukturierende Konsolidierung aus `ChatGPT_*` Notizen.
