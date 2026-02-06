---
title: Datenschutz (DSGVO) & anonyme Nutzung
---

## Verantwortliche & Impressum

Gib hier kurz die Verantwortlichen an (Name, Kontakt, ggf. Impressumsdaten). Dies ist ein Platzhalter — die endgültigen Angaben trägt das Projektteam ein.

## Erhobene Daten & Zwecke

- **Kerndaten Markierung:** Standort (geographisch), Titel, Beschreibung, Kategorie, optionales Foto/Anhang.
- **Betreiber‑Metadaten (Moderat.):** reports, audit logs (siehe `docs/concepts/moderation.md`).
- Zwecke: Veröffentlichung und Anzeige von Nutzerbeiträgen, Abuse‑Prevention, Qualitätsanalyse.

## Anonyme Abgabe

- Anonyme Einreichung ist möglich: keine Registrierung erforderlich; es wird maximal ein temporärer Client‑Token oder gehashte Meta‑Information gespeichert.
- Nutzer werden beim Einreichen darüber informiert, dass anonyme Beiträge weniger rekonstruierbar sind (keine Kontowiederherstellung).

## Betroffenenrechte (Auskunft, Löschung, Widerspruch)

- Nutzer können Auskunft über ihre personenbezogenen Daten verlangen und Löschung beantragen.
- Löschanfragen für Markierungen führen zur Entfernung der Darstellung; Moderations‑Metadaten können bis zur Retention‑Frist (90 Tage) aufgehoben archiviert sein, danach gelöscht oder anonymisiert.

## Retention & Anonymisierung

- Moderations‑Daten: **90 Tage** Aufbewahrung (siehe `docs/decisions/retention-moderation.md`).
- Operative Daten zur Anzeige von Markierungen werden nur so lange gehalten wie nötig; personenbezogene Metadaten werden anonymisiert, wenn möglich.

## Analytics & Cookies

- Empfehlung: cookieless, datenschutzfreundliche Analytics (Beispiel: Plausible oder self‑hosted Matomo mit anonymisierten IPs).
- Keine externe Tracking‑Skripte ohne ausdrückliche Prüfung und Transparenz.

## Externer Mapstack (Privacy‑Hinweis)

- Der Mapstack wird **nicht self‑hosted**: externe Vector‑Tile‑Provider werden verwendet. Dokumentiere Provider, deren Datenschutz‑Praxis und ggf. datenschutzfreundliche Konfigurationen (kein Referrer‑Leak, Tile‑CORS‑Einstellungen).

## Kontakt & Prozesse

- Kontaktinformationen für Datenschutzanfragen einfügen (E‑Mail / Postadresse).
- Prozesse zur Bearbeitung von Auskunfts‑ und Löschanfragen kurz beschreiben (SLA, Verantwortliche).

## Quellen

- Interne Notizen: ChatGPT_DSGVO.md, ChatGPT_Phase0_Erkenntnisse.md, ChatGPT_Moderation.md

## Impressum (Template)

Name / Organisation:
Anschrift:
Kontakt:
E‑Mail:

Füge hier die offiziellen Impressumsdaten des Projekteigentümers ein. Dies ist ein Platzhalter.

## Lösch‑ / Anonymisierungs‑Workflow (kurz)

1. Nutzer stellt Löschanfrage (E‑Mail oder Kontaktformular).
2. System entfernt die öffentliche Darstellung der Markierung innerhalb 48 Stunden (Sichtbarkeit).
3. Moderations‑Metadaten bleiben bis zu **90 Tagen** erhalten (Retention), werden danach gelöscht oder irreversibel anonymisiert.
4. Auskunfts‑/Löschanfragen antworten innerhalb von 14 Tagen (SLA); technische Löschung innerhalb 30 Tagen, abhängig von Backups und rechtlichen Anforderungen.

Hinweis: Bei berechtigten rechtlichen Anfragen (z. B. Behörden) können Ausnahmen gelten; dokumentiere solche Fälle gesondert.
