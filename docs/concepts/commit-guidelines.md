# Commit Richtlinien

Ich möchte meine Commits halbwegs einheitlich benennen und habe dafür das Projekt [Conventional Commits](https://www.conventionalcommits.org/de/v1.0.0/) kennengelernt.

Dieses Projekt soll also in Zukunft konventionelle Commit Nachrichten verwenden, nach dem Schema:

```text
<typ>[<gültigkeitsbereich>?]: <Beschreibung>

[optionaler Textkörper]

[optionale Fußnoten]
```

Aus den Namensgebung von fix, feat und BREAKING CHANGE folgt dann auch Semantic Versioning

## Konkrete Regeln (Kurz)

- `feat`: Neue Funktionalität
- `fix`: Bugfix
- `docs`: Änderungen an Dokumentation
- `style`: Formatierung, fehlende Semikolons, etc.
- `refactor`: Code‑Änderungen ohne funktionale Änderungen
- `test`: Tests hinzufügen/ändern
- `chore`: Build‑System oder Hilfswerkzeuge

Beispiel‑Commits

```
feat(map): add ghost-pin while placing a marking
fix(api): validate geometry in POST /markings
docs(readme): add setup instructions
```

CI / Enforcement

- Empfohlen: `commitlint` + `husky` für lokale Hooks, `commitlint` in CI zur Validierung von PR‑Titeln.
- Branch‑Naming: `feature/*`, `fix/*`, `hotfix/*`, `chore/*`.
- PR‑Beschreibung: Kurzbeschreibung, Verweis auf Issue (optional), Testplan (kurz).

Weitere Hinweise

- Für BREAKING CHANGES: füge `BREAKING CHANGE:` in den Body und erhöhe Major‑Version nach Semantic Versioning.
- Kleine Commits sind ok, aber jeder Commit sollte eine in sich abgeschlossene Änderung beschreiben.
