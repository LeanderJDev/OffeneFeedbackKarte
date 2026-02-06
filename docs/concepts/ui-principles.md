---
title: UI‑Principles & Add‑Marking Flow
---

Design‑Leitlinien

- Klarheit vor Feature‑Fülle: Fokus auf Kern‑Flow (Marking sehen → markieren → Kontext).
- Mobile‑First: einfache Gesten, grosses Tap‑Targets, optimierte Forms.
- Accessibility: ARIA‑Labels, kontrastreiche Farben, Tastaturzugang.

Map View und Controls

- Visible controls: Zoom, Kategorie‑Filter, Add‑Button
- Ghost‑Pin während Positionierung, Feedback über Snapping

Add‑Marking Flow

1. Start: Add Button → Ghost‑Pin angezeigt
2. Positionieren (Drag / Tap) → optional Snap an Straße
3. Formular: Titel (kurz), Beschreibung (optional), Foto (optional), Kategorie
4. Submit: Optimistic UI → Server‑Validierung

Detailpanel & Interaktionen

- Panel zeigt Title, Beschreibung, Votes, Comments, Zeitstempel
- Inline Actions: Vote, Comment, Report

Accessibility & mobile Verhalten

- Reduziere Modal‑Overload; nutze slide‑in Panels
- Unterstütze screen reader und large font sizes

Quellen: ChatGPT_Frontend_UX.md, ChatGPT_Phase0_Erkenntnisse.md
