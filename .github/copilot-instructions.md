<!-- Copilot / AI-Agent instructions for OffeneFeedbackKarte -->

# Kurzanleitung für KI-Coding-Agents

Ziel: Direkt einsetzbare, projekt-spezifische Hinweise für Code- und Dokumentations-Agents.

- **Big picture:** MVP besteht aus Frontend (Map UI) + Backend (REST API). Siehe Architektur-Notizen.
    - Frontend-Empfehlung: SvelteKit + MapLibre + Tailwind.
    - Backend-Empfehlung: FastAPI (Python).
    - Persistenz: PostgreSQL mit PostGIS für geodatenbasierte Queries.

- **Wo die Agent-Prompts leben:** `ai/agents/` (z. B. [ai/agents/docs-agent.md](ai/agents/docs-agent.md)). Benutze diese Dateien als source-of-truth für Rollen, Ziele und Einschränkungen.

- **Primäre Agent-Rollen (kurz):**
    - `Docs-Agent`: Dokumentation strukturieren, sprachlichen Ton anpassen. (siehe [ai/agents/docs-agent.md](ai/agents/docs-agent.md))
    - `UX/Concept-Agent`: UI-Alternativen, Accessibility, Map-Interaction-Design.
    - `Frontend-Agent`: SvelteKit-Komponenten, Map-Integration, Performance.
    - `Backend-Agent`: API-Design, DB-Schema (Pins, Votes, Kommentare), Sicherheit.
    - `Review-Agent`: Inkonsistenzen in Docs/Code, fehlende ADRs/Tests.

- **Agent-Verhalten — zwingend befolgen:**
    - Respektiere die `Einschränkungen` in den Agent-Prompts (z. B. Backend-Agent implementiert keine UI).
    - Nutze `ai/agents/*` nur als Referenz; verändere Basisprompts nur mit expliziter Zustimmung.
    - Vermeide autonome commits ohne PR und Review.

- **Praktische Hinweise / Beispiele aus dem Codebase:**
    - Dokumentation und Entscheidungen leben in `docs/` (Overview, ADRs, architecture). Prüfe [docs/README.md](docs/README.md) bevor du strukturelle Änderungen vorschlägst.
    - Prompt- und Agent-Design ist bereits in `ai/` dokumentiert — halte neuen Prompt-Entwurf in `ai/agents/` als eigene Datei.
    - Devlogs gehören in `docs/devlogs/` (wenn vorhanden) — benutze sie für reproduzierbare Experimente.

- **Konventionen & Erwartungen:**
    - Bevorzuge kurze, sachliche Formulierungen in Docs (siehe `ai/agents/docs-agent.md`).
    - Änderungen, die Architektur oder Datenmodell betreffen, brauchen eine ADR in `docs/decisions/`.
    - Tests / API-Dokumentation: Backend-API-Änderungen sollten OpenAPI/Markdown-Dokumentation erhalten.

- **Agent-Access & Sandboxing:**
    - Agenten sollten nicht automatisch Schreibzugriff auf `ai/agents/` haben; `ai/` dient als read-only prompt-reference.
    - Falls ein Agent Dateien liest, schränke die Pfade auf `src/`, `docs/` und `ai/agents/<relevant>` ein und explizit im Prompt nennen.
    - Ein einzelner Agent bezieht sich immer nur auf eine einzelne Rolle (z. B. Backend-Agent) und hat keinen Zugriff auf die Prompts anderer Agenten.

- **Tasks for Agents (concrete):**
    - Erstelle eine prägnante `docs/devlogs/26MMDD-...-ai-assisted.md` mit: Ziel, Prompt, Resultat, Learnings.

- **What not to do (explicit):**
    - Keine automatischen, unreviewed commits, besonders bei DB-/API-Änderungen.
    - Keine Änderung an ADRs ohne menschliche Bestätigung.
