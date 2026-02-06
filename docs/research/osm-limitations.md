---
title: OSM‑Limitierungen & Snapping‑Risiken
---

Hauptprobleme

- Heterogene Datenqualität: fehlende oder ungenaue Straßengeometrien in manchen Regionen.
- Unterschiedliche Tagging‑Konventionen führen zu Konsistenzproblemen beim Snapping.

Auswirkungen auf UX

- Fehl‑Snapping kann Nutzer verwirren; daher Originalkoordinate speichern und Snap‑Confidence anzeigen.

Mitigations

- Fallback: allow manual override; apply snapping only above zoom threshold.
- Use vector‑tiles with local augmentation where feasible (but extern gehostet).

Testfälle

- Regions­vergleich (City center vs Periphery)
- Edge‑cases: Fußwege, Plätze ohne named streets

Quellen: ChatGPT_Phase0_Erkenntnisse.md, ChatGPT_Existing_Projects.md
