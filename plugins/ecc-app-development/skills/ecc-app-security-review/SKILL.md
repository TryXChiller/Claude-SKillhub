---
name: ecc-app-security-review
description: Prüfe App-Änderungen gezielt auf Sicherheitslücken bei Anmeldung, Berechtigungen, Datenzugriff, Eingaben und Secrets. Nutze den Skill bei sicherheitsrelevanter Appentwicklung, API- oder Datenbankänderungen und vor einer Freigabe; für vollständige Repository-Audits den vorhandenen security-audit verwenden.
---

# App Security Review

Prüfe sachlich, risikoorientiert und mit konkreten Fundstellen. Unterscheide bestätigte Fehler, begründete Verdachtsfälle und nicht geprüfte Bereiche. Versprich keine vollständige Sicherheit.

## Vorgehen

1. Lies Projektanweisungen, den relevanten Diff und die betroffenen Aufrufer. Ermittle den tatsächlichen Stack, die Vertrauensgrenzen, Benutzerrollen und die Daten, die geschützt werden müssen. Setze Next.js, Supabase oder Docker nur voraus, wenn das Projekt sie tatsächlich verwendet.
2. Verfolge den betroffenen Ablauf vom Client über API und Autorisierung bis zur Speicherung. Ein ausgeblendeter UI-Button ist keine Zugriffskontrolle. Prüfe direkte API-Anfragen und alternative Zugriffswege.
3. Prüfe die relevanten Punkte unten. Priorisiere erreichbare Fehler mit konkreter Auswirkung; führe keinen vollständigen Audit bei jeder kleinen Änderung durch.
4. Reproduziere einen Verdacht möglichst mit vorhandenen lokalen Tests oder isolierten Testkonten. Begrenze Schreibzugriffe auf eigene Testdaten. Bereits erteilte Autorisierung beachten; keine produktiven Daten verändern oder externe Systeme angreifen, nur weil dieser Skill geladen wurde.
5. Liefere einen kleinen, gezielten Fix und einen aussagekräftigen Regressionstest, wenn die Aufgabe Änderungen umfasst. Bei einem reinen Review berichte die Befunde.

## Prüfschwerpunkte

- **Identität und Rechte:** Identität serverseitig aus einer verifizierten Sitzung ableiten. Vom Client gelieferte owner_id, tenant_id oder Rollen nicht als Berechtigungsnachweis akzeptieren. Lesen, Erstellen, Ändern und Löschen getrennt prüfen; auch erratene fremde Objekt-IDs und Bulk-Endpunkte berücksichtigen.
- **Mandantentrennung:** Mit zwei gewöhnlichen Testkonten prüfen, ob Konto A auf Daten von Konto B zugreifen kann. Ein Test mit einem Administrator beweist keine Isolation. Fehlende Testzugänge als offene Prüfung benennen.
- **Supabase/Postgres, falls vorhanden:** RLS-Policies und Grants pro betroffener Tabelle prüfen. INSERT/UPDATE sowohl auf zulässige Ausgangszeilen als auch auf zulässige neue Werte prüfen. Views und privilegierte Funktionen als mögliche alternative Zugriffswege betrachten. Service-Role-Zugänge umgehen RLS und gehören ausschließlich auf vertrauenswürdige Server; auch dort explizit autorisieren.
- **Eingaben und Ausgaben:** Serverseitige Validierung, parametrisierte Datenbankabfragen, kontextgerechtes Escaping und sichere Dateiverarbeitung prüfen. Bei benutzerkontrollierten URLs SSRF und erlaubte Ziele berücksichtigen. Keine pauschalen Sanitizer als Ersatz für sichere APIs empfehlen.
- **Sitzungen und Browser:** Cookie-Flags, CSRF-Schutz bei cookiegestützter Authentifizierung, CORS und Logout passend zum tatsächlichen Authentifizierungsfluss prüfen. SameSite nicht pauschal auf Strict setzen, wenn legitime Anmeldeabläufe dadurch brechen.
- **Secrets und Datenschutz:** Client-Bundles, Logs, Fehlermeldungen und versionierte Konfiguration auf sensible Werte prüfen. Befunde nur mit Pfad, Variablenname und redigiertem Kontext zeigen; keine Tokens, Passwörter, Auth-Dateien oder personenbezogenen Nutzdaten ausgeben. Öffentlich vorgesehene Schlüssel von privilegierten Schlüsseln unterscheiden. Bei realem Leak Rotation und Entfernung aus betroffenen Veröffentlichungen empfehlen.
- **Missbrauch und Betrieb:** Relevante Auth-, Upload- und kostspielige Endpunkte auf angemessene Limits prüfen. Abhängigkeiten im installierten Versionsstand bewerten; Scannerbefunde hinsichtlich Erreichbarkeit und Auswirkung einordnen. Keine automatischen Breaking-Upgrades oder audit-fix-Befehle als Nebenwirkung ausführen.
- **Selfhosting, falls vorhanden:** Exponierte Ports, Datenbankzugang, TLS, Containerrechte und dauerhafte Volumes prüfen, soweit vom geänderten Ablauf betroffen.

## Ergebnis

Führe zuerst den stärksten belegten Befund auf. Pro Befund nennen:
- Schweregrad und Status: bestätigt / Verdacht.
- Fundstelle und angreifbarer Ablauf, Voraussetzungen und konkrete Auswirkung.
- Kleinster sinnvoller Fix und passende Verifikation.

Schließe mit den durchgeführten Prüfungen und den wesentlichen offenen Punkten. Wenn nichts gefunden wurde, schreibe „In den geprüften Bereichen keine belegte Schwachstelle gefunden“ und benenne den Prüfbereich. Kein pauschales „sicher“.

## Herkunft

Adaptiert aus [ECC / security-review](https://github.com/affaan-m/ECC/blob/ef648e01899ba3e8dc6371642deaaf64b4477775/skills/security-review/SKILL.md), Stand `ef648e01899ba3e8dc6371642deaaf64b4477775`. Copyright (c) 2026 Affaan Mustafa, MIT-Lizenz; siehe `LICENSE`. Für diesen persönlichen Workflow überarbeitet. Keine automatische Aktualisierung oder Ausführung von Upstream-Code.
