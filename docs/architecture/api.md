---
title: API‑Spezifikation (Alpha → Beta)
---

Kurzüberblick

Dieses Dokument fasst die Kernendpunkte der Server‑API zusammen (Alpha‑Core und geplante Beta‑Erweiterungen). Ziel: klare Request/Response‑Beispiele, Fehlercodes und Hinweise zu Auth/Rate‑Limits.

Core Endpunkte

- GET /markings
    - Query: bbox (minLon,minLat,maxLon,maxLat), limit, offset, category, since
    - Response: Liste von Markings (id, geometry, title, description, category, created_at, snapped: bool)

- GET /markings/:id
    - Response: Detailliertes Marking (inkl. attachments, votes_count, comments_count)

- POST /markings
    - Auth: optional (anonymous submissions supported)
    - Body: { geometry: {lat,lon}, title, description, category, client_token? }
    - Response: 201 Created + Location header, Body: created resource

- POST /markings/:id/comments
    - Body: { text }
    - Response: 201 Created

- POST /markings/:id/votes
    - Body: { vote: 1 | -1 }
    - Response: 200 OK, updated counts

Supporting Endpunkte

- GET /health
    - Response: { status: "ok" }

- GET /users/:id (optional)
    - Minimale Darstellung: public profile, contribution counts

Fehler‑Handling

- Fehlerformat (JSON): { "error": { "code": "ERR_CODE", "message": "human readable", "details": {...} } }
- Gängige Codes: ERR_VALIDATION, ERR_AUTH, ERR_RATE_LIMIT, ERR_NOT_FOUND, ERR_INTERNAL

Authentifizierung & Rate‑Limits

- Auth: optional; API unterstützt anonymous submissions via `client_token`. Registrierte Nutzer über Bearer‑Token (JWT) für erweiterte Aktionen.
- Rate‑Limiting: IP/Fingerprint‑basierte Limits; aggressive Limits führen zu 429 ERR_RATE_LIMIT. Moderations‑Endpoints haben strengere Limits.

Moderation / Plausibilitätsprüfungen

- Server führt Plausibility‑Checks bei Einreichung durch: minimale Textlänge, Link‑Anteil, Häufigkeit pro BBox.
- Auffällige Einreichungen werden als `pending_review` markiert und im Report‑Workflow behandelt (siehe `docs/concepts/moderation.md`).

Beispiele

POST /markings (Request)

{
"geometry": { "lat": 52.5208, "lon": 13.4049 },
"title": "Defekte Straßenlaterne",
"description": "Seit Wochen defekt, Ecke Musterstraße",
"category": "infrastruktur"
}

Response: 201 Created

{
"id": "m_12345",
"status": "published",
"created_at": "2026-02-06T12:00:00Z"
}

Versionierung & Erweiterungen

- API‑Versionierung über Accept‑Header oder Pfad (`/v1/markings`).
- Beta‑Erweiterungen: bulk imports, advanced filtering, comments/votes enhancements.

Quellen

- Interne Notizen: ChatGPT_API_Spec.md, ChatGPT_SPEC_doc.md

## Fehler‑Response Schema (JSON)

Standardisiertes Fehlerformat (beispielsweise):

```json
{
    "error": {
        "code": "ERR_VALIDATION",
        "message": "Field 'title' is required",
        "details": { "field": "title", "reason": "required" }
    }
}
```

Gängige Fehlercodes (Beispiele):

- `ERR_VALIDATION` — Eingabefehler
- `ERR_AUTH` — Authentifizierungsfehler
- `ERR_RATE_LIMIT` — Rate Limit erreicht (HTTP 429)
- `ERR_NOT_FOUND` — Ressource nicht gefunden
- `ERR_INTERNAL` — Serverfehler

Antwortcodes:

- 200 OK — erfolgreiche Abfrage
- 201 Created — Ressource erstellt
- 400 Bad Request — Validierungsfehler
- 401 Unauthorized — fehlende/ungültige Auth
- 403 Forbidden — Berechtigung fehlt
- 404 Not Found — Ressource nicht gefunden
- 429 Too Many Requests — Rate Limit erreicht
- 500 Internal Server Error — unerwarteter Fehler

## Auth‑Flows

- **Anonymous submissions (client_token)**: Der Client kann einen temporären `client_token` (UUID oder signed token) mit dem Request senden, um wiederholte anonyme Aktionen vom selben Client zu erkennen.
- **Registered users (Bearer JWT)**: Registrierte Nutzer authentifizieren sich mit `Authorization: Bearer <jwt>`; Token enthält `sub` (user id) und scopes.

Beispiel Header (JWT):

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## Rate‑Limiting & Headers

- Server sendet bei limitierter Antwort HTTP 429 mit Headern:
    - `Retry-After: <seconds>`
    - `X-RateLimit-Limit: <limit>`
    - `X-RateLimit-Remaining: <remaining>`
    - `X-RateLimit-Reset: <unix-timestamp>`

- Empfehlung (Beispiel‑Policy):
    - Anonymous submissions: 60 requests / 10 minutes per fingerprint
    - Authenticated (registered): 300 requests / 10 minutes per user
    - Moderation endpoints: stricter e.g., 30 req / 10 minutes

## Hints für Implementierung

- Validierungen sollten konsistent sein (Shared validation layer). Fehler sollten immer den `error.code` enthalten, damit Clients deterministisch reagieren können.
- Bei batch/async‑Operations: Rückgabe eines `202 Accepted` mit `Location` auf den Task‑Status.
