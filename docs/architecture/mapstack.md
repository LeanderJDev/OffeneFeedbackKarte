---
title: Mapstack & Hosting‑Entscheidung
---

Entscheidung: Der Mapstack wird nicht self‑hosted betrieben; das Projekt verwendet externe Vector‑Tile‑Provider.

Gründe:

- Self‑hosting verursacht initial hohe Infrastruktur‑ und Betriebsaufwände.
- Externe Anbieter ermöglichen schnellen Start und Fokus auf Core‑Funktionalität.

Anforderungen an Provider:

- DSGVO‑konforme Datenverarbeitung und klare DPA/Vertrag.
- Möglichkeit, Referrer‑/Attributions‑Header zu konfigurieren, um Privacy‑Lecks zu minimieren.
- SLA‑/Kosten‑Übersicht und Fallback‑Strategie bei Ausfall.

Privacy‑Hinweis: Im `docs/concepts/privacy.md` wird auf die Nutzung externer Provider und die zu prüfenden Privacy‑Konfigurationen verwiesen.
