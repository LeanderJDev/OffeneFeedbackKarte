---
title: Datenbankschema (PostGIS)
---

Kurz: Dieses Dokument beschreibt das relationale Datenmodell mit PostGIS‑Erweiterung für die Kernentitäten der Plattform (Markings, Users, Comments, Votes, Files) sowie Moderations‑Metadaten und empfohlene Indizes.

Voraussetzungen

- PostgreSQL >= 13
- Extension: `postgis`

Tabellenübersicht

1. users

- `id` UUID PRIMARY KEY
- `created_at` TIMESTAMP WITH TIME ZONE
- `display_name` TEXT
- `email` TEXT NULL
- `is_anonymous` BOOLEAN DEFAULT false
- `metadata` JSONB

Hinweis: E‑Mail optional; anonyme Nutzung möglich (kein zwingender Account‑Flow).

2. markings

- `id` UUID PRIMARY KEY
- `user_id` UUID REFERENCES users(id) NULL
- `geom` GEOMETRY(POINT, 4326) NOT NULL
- `snapped_geom` GEOMETRY(POINT, 4326) NULL -- wenn Snapping angewendet wurde
- `title` TEXT
- `description` TEXT
- `category` TEXT
- `status` TEXT DEFAULT 'published' -- values: published|pending_review|hidden|deleted
- `attachments` JSONB NULL
- `properties` JSONB NULL
- `created_at` TIMESTAMP WITH TIME ZONE
- `updated_at` TIMESTAMP WITH TIME ZONE

3. comments

- `id` UUID PRIMARY KEY
- `marking_id` UUID REFERENCES markings(id) ON DELETE CASCADE
- `user_id` UUID REFERENCES users(id) NULL
- `text` TEXT
- `created_at` TIMESTAMP WITH TIME ZONE
- `is_moderated` BOOLEAN DEFAULT false

4. votes

- `id` UUID PRIMARY KEY
- `marking_id` UUID REFERENCES markings(id) ON DELETE CASCADE
- `user_id` UUID REFERENCES users(id) NULL
- `fingerprint` TEXT NULL -- optional, wenn anonym
- `vote` SMALLINT -- 1 or -1
- `created_at` TIMESTAMP WITH TIME ZONE

Constraint: prefer unique(user_id, marking_id) for registered users; for anonymous, use `fingerprint` uniqueness.

5. files

- `id` UUID PRIMARY KEY
- `marking_id` UUID REFERENCES markings(id) ON DELETE CASCADE
- `storage_path` TEXT
- `content_type` TEXT
- `size` INTEGER
- `created_at` TIMESTAMP WITH TIME ZONE

6. reports (Moderation)

- `id` UUID PRIMARY KEY
- `marking_id` UUID REFERENCES markings(id)
- `reporter_meta` JSONB -- z.B. anonymized fingerprint, note
- `reason` TEXT
- `spam_score` NUMERIC NULL
- `status` TEXT DEFAULT 'open' -- open|in_review|resolved
- `created_at` TIMESTAMP WITH TIME ZONE

7. moderator_actions / audit_logs

- `id` UUID PRIMARY KEY
- `report_id` UUID REFERENCES reports(id) NULL
- `target_table` TEXT -- e.g., 'markings'
- `target_id` UUID -- id of the affected row
- `action` TEXT -- e.g., 'delete','quarantine','warn'
- `moderator_id` UUID REFERENCES users(id) NULL
- `reason` TEXT
- `created_at` TIMESTAMP WITH TIME ZONE

Wichtige Indizes

- GIST index auf `markings.geom`: `CREATE INDEX ON markings USING GIST (geom);`
- GIST auf `markings.snapped_geom` falls verwendet
- Btree auf `markings.created_at`, `markings.category`, `markings.status`
- GIN index für Volltextsuche: `CREATE INDEX ON markings USING GIN (to_tsvector('german', coalesce(title,'') || ' ' || coalesce(description,'')));`
- Index auf `reports.created_at` und `reports.status`

Beispiel‑Queries

- Bounding‑Box Abfrage (liefert Markings innerhalb bbox):

```sql
SELECT * FROM markings
WHERE ST_Intersects(geom, ST_MakeEnvelope(:minLon, :minLat, :maxLon, :maxLat, 4326))
AND status = 'published'
ORDER BY created_at DESC
LIMIT :limit OFFSET :offset;
```

- Snapping: nearest road point (Pseudocode):

```sql
-- Annahme: roads is a table with geometries
SELECT r.id, ST_ClosestPoint(r.geom, m.geom) AS snapped_point
FROM roads r
ORDER BY ST_Distance(r.geom, m.geom)
LIMIT 1;
```

Migrationsempfehlungen

- PostGIS Extension zuerst anlegen: `CREATE EXTENSION IF NOT EXISTS postgis;`
- Indizes `CONCURRENTLY` erstellen, wenn möglich, um Downtime zu vermeiden.
- Bei Hinzufügen großer Spalten (z. B. JSONB) testen und ggf. in Backfill‑Batches migrieren.

Retention & Anonymisierung

- Moderations‑Daten (`reports`, `moderator_actions`, ggf. `audit_logs`) werden für **90 Tage** aufbewahrt. Nach Ablauf: Löschung oder irreversible Anonymisierung.
- Logs für Debugging (z. B. request logs) sollten gesondert behandelt werden und sind nicht Teil dieses Schemas.

Hinweise

- Verwende UUIDs als Primärschlüssel für einfache Replikation und bessere Privacy‑Praktiken.
- Trenne Anzeige‑Daten (Markings) von Moderations‑Metadaten technisch und durch Zugriffsrechte.

Quellen

- ChatGPT_DB_Scheme.md, ChatGPT_API_Spec.md, ChatGPT_Phase0_Erkenntnisse.md
