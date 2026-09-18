# tryxchiller-skills

Meine persönliche Claude-Code-Plugin-Marketplace. Ein Repo, das ich auf jedem
Rechner (Laptop, tryxlenovo, ...) hinzufüge, statt Skills manuell zu kopieren.

## Struktur

```
tryxchiller-skills/
├── .claude-plugin/
│   └── marketplace.json          ← Katalog: listet alle Plugins hier
└── plugins/
    └── security-audit/           ← ein Plugin = ein Ordner
        ├── .claude-plugin/
        │   └── plugin.json       ← Metadaten für dieses Plugin
        └── skills/
            └── security-audit/   ← der eigentliche Skill
                ├── SKILL.md
                └── ... (Referenzdateien)
```

Bereits enthalten: `security-audit` (Cloudflare-Skill für Security-Audits).

## Einmalig: Repo auf GitHub anlegen

1. Auf github.com ein neues, privates Repo anlegen, z. B. `tryxchiller-skills`
   (leer lassen, keine README/License von GitHub generieren lassen).
2. In diesem Ordner hier:

   ```bash
   cd tryxchiller-skills
   git init
   git add .
   git commit -m "Initial marketplace: security-audit"
   git branch -M main
   git remote add origin https://github.com/DEIN-USERNAME/tryxchiller-skills.git
   git push -u origin main
   ```

## Auf jedem Rechner (Laptop, tryxlenovo, ...) einmalig

In Claude Code (Terminal oder VS-Code-Extension):

```
/plugin marketplace add DEIN-USERNAME/tryxchiller-skills
/plugin install security-audit@tryxchiller-skills
```

Falls danach `Run /reload-plugins to activate.` erscheint, das ausführen.
Testen mit:

```
/security-audit:security-audit
```

## Weitere Skills hinzufügen (z. B. deine anderen Terminal-Skills)

Für jeden zusätzlichen Skill, den du aktuell nur lokal unter
`~/.claude/skills/<name>/` auf einem Rechner liegen hast:

```bash
# 1. Plugin-Ordner anlegen
mkdir -p plugins/<name>/.claude-plugin
mkdir -p plugins/<name>/skills

# 2. Skill-Ordner reinkopieren (vom Rechner, auf dem er aktuell liegt)
cp -r ~/.claude/skills/<name> plugins/<name>/skills/<name>

# 3. plugin.json anlegen (Vorlage unten)
```

`plugins/<name>/.claude-plugin/plugin.json`:
```json
{
  "name": "<name>",
  "description": "<kurze Beschreibung>",
  "version": "1.0.0",
  "author": { "name": "TryXChiller" }
}
```

Dann in `.claude-plugin/marketplace.json` einen weiteren Eintrag im
`"plugins"`-Array ergänzen (Kopie des `security-audit`-Eintrags, Name und
`source`-Pfad anpassen).

Committen und pushen:
```bash
git add .
git commit -m "Add <name>"
git push
```

Auf den anderen Rechnern reicht danach:
```
/plugin marketplace update tryxchiller-skills
/plugin install <name>@tryxchiller-skills
```

## Updates einspielen

Nach jedem Push auf einem anderen Rechner:
```
/plugin marketplace update tryxchiller-skills
/plugin update security-audit@tryxchiller-skills
```
(Versionsnummer in der jeweiligen `plugin.json` hochzählen, sonst wird das
Update evtl. nicht erkannt.)
