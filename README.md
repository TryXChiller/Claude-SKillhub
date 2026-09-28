# tryxchiller-skills

Meine persönliche Claude-Code-Plugin-Marketplace. Ein Repo, das ich auf jedem
Rechner (Laptop, tryxlenovo, ...) hinzufüge, statt Skills manuell zu kopieren.

## Enthaltene Plugins

- `security-audit`: sechsphasiger Workflow für Sicherheitsprüfungen
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
```

gstack selbst wird anschließend erneut über
`/gstack-installer:install-gstack` aktualisiert.
