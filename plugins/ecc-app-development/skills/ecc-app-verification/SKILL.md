---
name: ecc-app-verification
description: Verifiziere App-Änderungen vor der Fertigmeldung mit passenden Build-, Typ-, Lint-, Test- und Laufzeitprüfungen. Nutze den Skill nach Implementierungen oder Fehlerbehebungen und bei der Frage, ob eine Funktion wirklich funktioniert oder für eine Freigabe bereit ist.
---

# App Abschlussprüfung

Belege die behauptete Funktion mit passenden Prüfungen. Ein erfolgreicher Build beweist weder einen funktionierenden Nutzerablauf noch dauerhafte Speicherung.

## Prüfplan aus dem Projekt ableiten

1. Lies Projektanweisungen, den Diff, vorhandene Testkonfigurationen, CI und Paketmanager-Lockdateien. Nutze vorhandene Skripte und das passende Werkzeug; erfinde keine Standardbefehle, die das Projekt nicht anbietet.
2. Formuliere kurz das betroffene Nutzerverhalten und die verbleibenden Risiken. Wähle die kleinste aussagekräftige Kombination vorhandener Qualitätsprüfungen und gezielter Funktionstests. Beachte verpflichtende Projekt-Gates. Erzwinge keine beliebige Coverage-Quote.
3. Prüfe zuerst günstige relevante Gates, dann gezielte Tests und nötige Laufzeitabläufe. Führe Tests nur parallel aus, wenn sie weder Build-Ausgaben, Testdaten, Ports noch andere veränderliche Ressourcen teilen.

## Ausführung und Beweiskraft

- Halte den tatsächlichen Exit-Code jedes Prozesses fest. Verlasse dich nicht auf den Status von `head`, `tail` oder `tee` in einer Pipeline. Falls eine Pipeline nötig ist, verwende im passenden Shell-Kontext pipefail bzw. erfasse den ursprünglichen Prozessstatus. Prüfe die vollständigen relevanten Fehlermeldungen, bevor du sie zusammenfasst.
- Trenne „bestanden“, „fehlgeschlagen“, „blockiert“ und „nicht ausgeführt“. Ein fehlender Zugang, eine nicht gestartete Datenbank oder ein Timeout ist kein bestandener Test. Nenne Ursache und nächsten konkreten Schritt.
- Bei Fehlern: Ursache untersuchen, aufgabenbezogen beheben, betroffene Prüfung wiederholen. Tests, Assertions oder Qualitätsregeln nicht abschwächen, nur um Grün zu erhalten. Vorbestehende Fehler nur dann so bezeichnen, wenn dafür Vergleichsevidenz vorliegt.
- Prüfe API-, Auth- und Speicheränderungen über ihren tatsächlichen Pfad. Ein Screenshot oder lokaler State allein reicht nicht. Unterscheide gemockte Tests von Prüfungen mit echtem Backend.
- Bei dauerhaften Daten: erfolgreiche Serverantwort und wiederholtes Abrufen derselben Datensatz-ID nach neuer Anmeldung bzw. in einem frischen Browserkontext prüfen. Ein HTTP 200 allein beweist keine Speicherung. Für mehrschrittige Nutzerabläufe `ecc-app-e2e-testing` hinzuziehen.
- Bei sicherheitsrelevanten Änderungen `ecc-app-security-review` anwenden. Einen allgemeinen Security-Audit nur bei passender Aufgabenstellung ausweiten.
- Logs und Testartefakte ohne Secrets oder unnötige personenbezogene Daten halten. Keine .env-Inhalte oder Sitzungsdateien in Ergebnisberichte kopieren.
- Keine Deployments, produktiven Migrationen, Dependency-Upgrades oder destruktiven Bereinigungen allein zur Abschlussprüfung auslösen. Nutze die für die Aufgabe autorisierte Umgebung.

## Abschluss

Prüfe den finalen Diff auf versehentliche Änderungen, Debug-Ausgaben und unvollständige Platzhalter. Wiederhole nach Korrekturen nur Prüfungen, deren Ergebnis durch diese Änderungen ungültig wurde, sowie zwingende Projekt-Gates.

Berichte knapp:
1. Was geändert wurde und welches Verhalten jetzt belegt ist.
2. Welche Prüfungen tatsächlich liefen und ihr Ergebnis; bei Bedarf eine kleine Tabelle mit Prüfung, Status und Aussagekraft.
3. Was offen oder blockiert bleibt und welche Auswirkung das auf die Fertigmeldung hat.

Kennzeichne einen nur teilweise geprüften Stand ausdrücklich. Behaupte weder erfolgreiche E2E-Verifikation noch Produktionsreife, wenn nur statische Prüfungen oder Mocks gelaufen sind. Wenn genügend Evidenz für den beauftragten Umfang vorliegt, beende die Prüfung ohne zusätzliche Schleifen.

## Herkunft

Adaptiert aus [ECC / verification-loop](https://github.com/affaan-m/ECC/blob/ef648e01899ba3e8dc6371642deaaf64b4477775/skills/verification-loop/SKILL.md), Stand `ef648e01899ba3e8dc6371642deaaf64b4477775`. Copyright (c) 2026 Affaan Mustafa, MIT-Lizenz; siehe `LICENSE`. Für diesen persönlichen Workflow überarbeitet. Keine automatische Aktualisierung oder Ausführung von Upstream-Code.
