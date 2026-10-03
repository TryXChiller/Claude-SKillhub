# tryxchiller-skills

Meine persönliche Claude-Code-Plugin-Marketplace. Ein Repo, das ich auf jedem
Rechner (Laptop, tryxlenovo, ...) hinzufüge, statt Skills manuell zu kopieren.

## Enthaltene Plugins

- `security-audit`: sechsphasiger Workflow für Sicherheitsprüfungen
- `ecc-app-development`: gezielter App-Security-Review, Abschlussprüfung und E2E-Tests
- `vermenschlichen`: deutsche Texte natürlich und leserorientiert überarbeiten
- `gstack-installer`: installiert oder aktualisiert Garry Tans
  [gstack](https://github.com/garrytan/gstack) über dessen offizielles Setup

## Auf jedem Rechner einmalig

In Claude Code oder der VS-Code-Erweiterung:

```
/plugin marketplace add TryXChiller/Claude-SKillhub
```

Danach die gewünschten Plugins installieren:

```
/plugin install security-audit@tryxchiller-skills
/plugin install vermenschlichen@tryxchiller-skills
/plugin install gstack-installer@tryxchiller-skills
/plugin install ecc-app-development@tryxchiller-skills
```

Falls Claude anschließend `Run /reload-plugins to activate.` anzeigt, diesen
Befehl ausführen.

gstack wird danach über den Installer-Skill eingerichtet:

```
/gstack-installer:install-gstack
```

Der Installer lädt gstack direkt aus dem offiziellen Repository. Dadurch bleiben
die umfangreichen gstack-Dateien außerhalb dieses Marketplaces und können mit dem
Originalprojekt aktualisiert werden. Voraussetzungen sind Git, Bun 1.0+ und
Node.js; unter Windows wird Git Bash oder WSL verwendet.

## Struktur

```
tryxchiller-skills/
├── .claude-plugin/
│   └── marketplace.json
└── plugins/
    ├── security-audit/
    ├── vermenschlichen/
    ├── ecc-app-development/
    └── gstack-installer/
        ├── .claude-plugin/
        │   └── plugin.json
        └── skills/
            └── install-gstack/
                └── SKILL.md
```

## Aktualisieren

Den Marketplace und installierte Plugins in Claude Code aktualisieren:

```
/plugin marketplace update tryxchiller-skills
/plugin update security-audit@tryxchiller-skills
/plugin update vermenschlichen@tryxchiller-skills
/plugin update gstack-installer@tryxchiller-skills
/plugin update ecc-app-development@tryxchiller-skills
```

gstack selbst wird anschließend erneut über
`/gstack-installer:install-gstack` aktualisiert.

## ECC für die Appentwicklung

`ecc-app-development` enthält drei für unseren Workflow angepasste Skills:

| Skill | Aufgabe |
| --- | --- |
| `ecc-app-security-review` | Anmeldung, Rechte, Mandantentrennung und Datenzugriff gezielt prüfen |
| `ecc-app-verification` | Build, Tests und tatsächliches Verhalten vor der Fertigmeldung belegen |
| `ecc-app-e2e-testing` | Nutzerabläufe und dauerhafte Speicherung nach erneuter Anmeldung prüfen |

Die Beschreibungen ermöglichen eine aufgabenabhängige Auswahl durch Claude.
Expliziter Aufruf:

```
/ecc-app-development:ecc-app-security-review
/ecc-app-development:ecc-app-verification
/ecc-app-development:ecc-app-e2e-testing
```

Der bestehende `security-audit` bleibt für vollständige Sicherheitsprüfungen
verfügbar. Die neuen Skills ergänzen ihn für einzelne Entwicklungsaufgaben.

Grundlage: [ECC](https://github.com/affaan-m/ECC), festgehaltener Stand
`ef648e01899ba3e8dc6371642deaaf64b4477775`. Die drei Anleitungen wurden angepasst;
Build-Erfolg, gemockte Tests und echte Backend-Verifikation werden ausdrücklich
unterschieden. MIT-Lizenz und Quellenangaben liegen bei.

Dieses Plugin enthält ausschließlich Anleitungen und Lizenztexte. Es richtet
keine Hooks, MCP-Server oder automatischen Upstream-Updates ein und führt bei der
Installation keine ECC-Skripte aus. Bei Verwendung können die Skills Claude zu
aufgabenbezogenen Prüfungen mit den bereits freigegebenen Werkzeugen anleiten.
