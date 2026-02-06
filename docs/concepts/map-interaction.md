---
title: Karteninteraktion & Snapping
---

Snapping‑Philosophie

- Snapping verbessert Präzision bei heterogenen OSM‑Daten; Originalkoordinate speichern plus `snapped_geom`.
- Snapping nur innerhalb sinnvoller Zoom‑Größen und mit konfigurierbarer `snap_distance` (Default TBD).

Live‑Snapping Algorithmus (Übersicht)

- Client berechnet nearest‑road candidate oder fragt Server‑Endpoint `/snap`.
- Server verwendet spatial index (PostGIS) oder Vector‑Tile‑Based nearest point.
- Bei Unsicherheit: Anzeige von Confidence und Möglichkeit manueller Korrektur.

Viewport‑Validation Regeln

- Verwerfe Submits, wenn Marking weit außerhalb aktuellem viewport liegt (Anti‑spam Maßnahme).

Vector vs Raster Tiles

- Empfehlung: externe Vector‑Tile Provider (MapLibre kompatibel) für Styling‑Flexibilität; nicht self‑hosted.

Client‑fetch & Performance

- Prefetch visible tile ids; limit requests per interaction; use debounce on search/snapping.

Quellen: ChatGPT_API_Spec.md, ChatGPT_Phase0_Erkenntnisse.md, ChatGPT_Frontend_UX.md
