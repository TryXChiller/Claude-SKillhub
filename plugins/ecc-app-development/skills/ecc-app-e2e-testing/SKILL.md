---
name: ecc-app-e2e-testing
description: Erstelle und prüfe aussagekräftige End-to-End-Tests für App-Nutzerabläufe mit vorhandenen Browserwerkzeugen, bevorzugt der bestehenden Playwright-Suite. Nutze den Skill bei Anmeldung, CRUD-Abläufen, Speicherproblemen, verschwundenen Daten nach erneutem Öffnen und wichtigen UI-API-Datenbank-Integrationen.
---

# App E2E-Tests

Prüfe sichtbares Nutzerverhalten und die dazugehörigen Systemgrenzen. Nutze bestehende Testwerkzeuge, Skripte und Konventionen; installiere kein zusätzliches Framework, wenn das vorhandene genügt.

## Ablauf festlegen

1. Lies die betroffenen Oberflächen, API-Aufrufe, Speicherung und vorhandenen Tests. Bestimme die Testumgebung, Rollen, Seed-Daten und Aufräummöglichkeiten.
2. Formuliere einen konkreten Nutzerablauf mit beobachtbarem Ziel und relevanten Fehlerfällen. Schreibe bevorzugt wenige belastbare Tests für den geänderten Ablauf statt einer großen redundanten Suite.
3. Verwende eigene, eindeutig markierte Testdatensätze und isolierte Testkonten. Parallel laufende Tests dürfen sich keine veränderlichen Datensätze teilen. Lösche beim Aufräumen ausschließlich vom Test angelegte Daten.

## Dauerhafte Speicherung belegen

Wenn der Nutzer Daten dauerhaft oder geräteübergreifend speichern will, prüfe beispielsweise:
1. Als gewöhnlicher Testnutzer einen Kunden oder Zeiteintrag mit eindeutiger Kennung anlegen.
2. Die tatsächliche Speicheranfrage samt erfolgreicher Antwort prüfen und die serverseitige Datensatz-ID erfassen. Den Inhalt durch einen erneuten autorisierten Abruf verifizieren.
3. Den ersten Browserkontext schließen. Einen frischen Kontext ohne übernommenes localStorage, IndexedDB, Cache oder gespeicherten App-State starten; normal erneut anmelden.
4. Denselben Datensatz anhand seiner ID und relevanter Felder wiederfinden. Falls ein zweites Gerät nicht verfügbar ist, den frischen Kontext als Näherung kennzeichnen, nicht als echten Gerätetest ausgeben.
5. Änderungen nach erneutem Laden bzw. erneutem Abruf prüfen; Löschen nur mit eigenen Testdaten prüfen.

Eine identische UI nach Reload kann nur lokalen Cache belegen. Ein gemockter Speicher-Endpunkt kann UI-Verhalten prüfen, aber keine echte Backend-Persistenz. Benenne diese Grenze ausdrücklich. Offline-first-Funktionen getrennt auf lokalen Zustand, ausstehende Synchronisierung und erfolgreich bestätigte Serverspeicherung prüfen.

## Robuste Browser-Tests

- Bevorzuge Rollen, Labels und stabile Test-IDs statt fragiler CSS-Ketten. Nutze Locator-Auto-Waiting und wiederholende Assertions.
- Warte auf konkrete sichtbare Zustände oder relevante Antworten statt fester Schlafzeiten oder pauschalem networkidle.
- Registriere eine erwartete Response oder Navigation vor der auslösenden Aktion, damit schnelle Ereignisse nicht verloren gehen. Ordne die Antwort dem konkreten Endpunkt und Request zu, nicht irgendeinem Status 200.
- Prüfe Inhalt und Zustandsänderung, nicht nur die Existenz eines Elements oder einen Screenshot. Prüfe, dass ein Klick die beabsichtigte Wirkung auslöst.
- Nutze unterstützte Auth-Fixtures, wenn passend. Speichere auth storageState, Tokens und Cookies außerhalb versionierter Artefakte. Für den Persistenztest kein Fixture verwenden, das gespeicherte App-Daten mitnimmt.
- Sichere bei Fehlschlägen nur nötige Traces, Screenshots und redigierte Logs. Beachte, dass auch HAR-Dateien und Traces Zugangsdaten enthalten können.
- Untersuche flakige Tests. Ein erfolgreicher Retry muss als Retry erkennbar bleiben. Tests nicht stillschweigend mit skip/fixme aus der Erfolgsbewertung entfernen; eine bewusst quarantänisierte Prüfung als Lücke nennen.

## Fehlerfälle passend zum Risiko

Prüfe bei Speicheränderungen abgelaufene Anmeldung, verweigerte Rechte, Serverfehler und Verbindungsabbruch. Die UI darf fehlgeschlagene Speicherung nicht als erfolgreich darstellen; Eingaben sollen soweit vorgesehen wiederherstellbar sein. Bei Wiederholung oder Doppelklick prüfen, ob unerwünschte Duplikate entstehen. Bei konkurrierenden Änderungen die projektseitig gewählte Konfliktregel prüfen.

Prüfe bei Mandantentrennung mit zwei normalen Testkonten, dass fremde Datensätze über direkte API-Aufrufe nicht zugänglich sind. Ein verstecktes Element reicht als Nachweis nicht.

Verwende Fehler-Injektion oder Mocks zur kontrollierten Prüfung von Fehlerzuständen, benenne diese Tests aber getrennt von echten Integrationsprüfungen. Keine Störungen oder Testschreibvorgänge in Produktion ohne entsprechende Autorisierung.

## Ergebnis

Nenne getesteten Ablauf, Umgebung, echte und gemockte Systemteile, Ergebnis und verbleibende Lücken. Falls Backend, Browser oder Testzugänge fehlen, liefere die mögliche Prüfung und passende Tests; kennzeichne die Laufzeitverifikation als blockiert. Behaupte niemals, ein ungestarteter oder übersprungener Test sei bestanden.

## Herkunft

Adaptiert aus [ECC / e2e-testing](https://github.com/affaan-m/ECC/blob/ef648e01899ba3e8dc6371642deaaf64b4477775/skills/e2e-testing/SKILL.md), Stand `ef648e01899ba3e8dc6371642deaaf64b4477775`. Copyright (c) 2026 Affaan Mustafa, MIT-Lizenz; siehe `LICENSE`. Für diesen persönlichen Workflow überarbeitet. Keine automatische Aktualisierung oder Ausführung von Upstream-Code.
