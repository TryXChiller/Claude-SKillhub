---
name: install-gstack
description: Installiert, repariert oder aktualisiert Garry Tans gstack fuer Claude Code. Verwenden, wenn der Nutzer gstack einrichten, aktualisieren, auf einem neuen Rechner verfuegbar machen oder eine fehlerhafte gstack-Installation reparieren moechte.
---

# gstack installieren

Installiere gstack aus dem offiziellen Repository `https://github.com/garrytan/gstack.git`.

## Vorgehen

1. Pruefe, ob Git, Bun 1.0+ und Node.js vorhanden sind. Unter Windows Git Bash oder WSL verwenden.
2. Verwende als Ziel `~/.claude/skills/gstack`.
3. Wenn das Ziel noch nicht existiert, fuehre aus:

   ```bash
   git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
   cd ~/.claude/skills/gstack
   ./setup
   ```

4. Wenn das Ziel bereits existiert, pruefe zuerst mit `git remote get-url origin`, dass es wirklich auf `garrytan/gstack` zeigt. Bei einem anderen Remote oder einem Verzeichnis ohne Git-Metadaten stoppen und nichts loeschen oder ueberschreiben.
5. Eine bestaetigte Installation so aktualisieren:

   ```bash
   git -C ~/.claude/skills/gstack pull --ff-only origin main
   cd ~/.claude/skills/gstack
   ./setup
   ```

6. Das Setup-Ergebnis pruefen und dem Nutzer fehlende Voraussetzungen oder betroffene Skills klar nennen.
7. Telemetrie nicht eigenmaechtig aktivieren.
8. `CLAUDE.md` oder ein aktuelles Projekt nur auf ausdruecklichen Wunsch aendern. Team-Modus und Commits ebenfalls vorher mit dem Nutzer abstimmen.

Nach erfolgreichem Setup eine neue Claude-Code-Sitzung starten oder die Plugins/Skills neu laden und beispielsweise `/office-hours`, `/review` oder `/qa` testen.
