# Decisions / ADRs

In diesem Ordner werden Architecture Decision Records (ADRs) abgelegt — formale Kurz‑Dokumente, die wichtige Architektur‑ oder Projektentscheidungen festhalten.

Wann eine ADR anlegen?

- Wenn eine längerfristige, nicht triviale Entscheidung getroffen wird (z. B. Lizenz, Hosting, Datenschutz‑Policy).

Konventionen

- Dateiname: `0001-<short-title>.md` für nummerierte ADRs.
- Inhalt: `Title`, `Date`, `Status` (proposed/accepted/declined), `Context`, `Decision`, `Consequences`, `Links`.

Beispiel‑Template

```md
Title: Lizenz‑Wahl (AGPLv3 empfohlen)
Date: 2026-02-06
Status: proposed

Context:
Kurz erläutern, warum eine Entscheidung nötig ist.

Decision:
AGPLv3

Consequences:
Auswirkungen auf Nutzung, Contribution, Hosting.

Links:

- docs/architecture/SPEC.md
```

Wie verlinken?

- Verweise relevante ADRs in `docs/README.md` oder in spezifischen Specs (z. B. `docs/architecture/SPEC.md`).

Pflege

- Aktualisiere Status und vermerke Follow‑ups als neue ADRs; lösche keine ADRs, archiviere stattdessen Updates.
