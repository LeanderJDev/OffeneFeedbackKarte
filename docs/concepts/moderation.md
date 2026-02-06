---
title: Moderation, Reporting & Spam‑Protection
---

## Moderationsprinzipien

- Offen, nachvollziehbar und verhältnismäßig: Moderationsentscheidungen dokumentieren und auditierbar machen.
- Minimale Automatisierung: Automatische Maßnahmen (z. B. Rate‑limits, heuristische Filter) kennzeichnen und zur manuellen Prüfung weiterleiten.
- Schutz der Community: Schnelle Reaktion bei sicherheitsrelevanten Inhalten.

## Technische Maßnahmen

- **Rate‑Limiting:** IP/Fingerprint‑basierte Limits pro Zeitfenster; konservative Defaults; Eskalation nur nach Review.
- **Fingerprinting:** Hash(zeitlich gefilterte IP + UserAgent + optional ClientToken) zur kurzfristigen Abuse‑Erkennung.
- **Captcha / Throttling:** Aktivierung bei auffälligem Verhalten (z. B. Massen‑Submits).
- **Plausibilitätsprüfungen:** Inhaltlänge, Link‑Anteil, Submit‑Frequenz pro Bounding Box.

## Reporting‑Workflow (Kurzbeschreibung)

1. Nutzer meldet Markierung → `report`-Eintrag (ID, marking_id, reporter_meta) wird erzeugt.
2. Automatische Vorqualifikation (Spam‑Score) und Priorisierung (hoch/medium/low).
3. Moderator prüft: Optionen: Löschen, Sichtbarmachen, Quarantäne, Nutzerwarnung, Shadowban.
4. Maßnahmen werden auditiert (wer, wann, warum) und die Metadaten mit Aufbewahrungsfrist gespeichert.

## Admin‑/Moderator‑Rollen

- Rollen minimal halten: `viewer`, `moderator`, `admin`.
- Zugriff auf sensible Metadaten ist strikt eingeschränkt und protokolliert.

## Moderationsdaten & Retention

- Alle Moderations‑Metadaten (reports, actions, audit logs) werden für **90 Tage** aufbewahrt.
- Nach 90 Tagen erfolgt vollständige Löschung oder irreversible Anonymisierung (Hashing ohne Wiederherstellung).
- Ausnahmen nur bei rechtlicher Verpflichtung; solche Fälle sind separat zu dokumentieren.

## Automatisierung und Machine‑Assisted Moderation

- Heuristiken dürfen Prioritäten vorschlagen, nicht aber endgültige Löschentscheidungen treffen.
- Shadowban‑Mechanik ist zulässig, muss aber dokumentiert, überprüfbar und befristet sein.

## Datenschutz & Minimierung

- Nur notwendige Metadaten speichern; vollständige IP‑Adressen vermeiden (nur Kurz‑Hashes oder truncation).
- Verweise auf `docs/concepts/privacy.md` für Betroffenenrechte, Löschprozesse und Anonymisierungsflüsse.

## Offene Punkte

- Moderations‑UI (konkrete Screens/Workflows) fehlt noch.
- Thresholds für Spam‑Score und automatische Maßnahmen müssen per Tests definiert werden.

## Moderator Quick‑Reference

- **Statuswerte:** `open`, `in_review`, `resolved`, `escalated`.
- **Mögliche Aktionen:** `dismiss` (kein Handlungsbedarf), `hide` (verstecke Marking), `delete` (permanent löschen), `quarantine` (temporär verbergen), `warn_user`, `shadowban`.
- **Empfohlene Reihenfolge bei Report:** 1) Sichtprüfung 2) Spam‑Check 3) Aktion dokumentieren 4) Notifiy reporter (optional).
- **Audit:** Jede Aktion erzeugt einen `moderator_action`‑Eintrag (moderator_id, action, reason, timestamp).

## Beispiel: Report Payload (JSON)

```json
{
    "id": "r_123",
    "marking_id": "m_12345",
    "reporter_meta": { "fingerprint": "abc123" },
    "reason": "offensive content",
    "spam_score": 0.85,
    "status": "open",
    "created_at": "2026-02-06T12:34:00Z"
}
```

## Escalation & Thresholds (Vorschlag)

- `spam_score >= 0.9` → Auto‑queue als `high` priority für Moderator, possible auto‑quarantine.
- `reports_count >= 3` innerhalb 7 Tagen → Escalate und erhöhe Priorität.
