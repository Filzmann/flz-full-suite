# Betriebs- und Rückbauhandbuch der AD-Suite

Dieses Handbuch ergänzt die Installationsanleitung. Es gilt für einzeln oder
gemeinsam ausgelieferte AD-Fachprodukte auf der durch den konkreten
Releasekandidaten nachgewiesenen Nextcloud-Zielmajor mit PHP ab 8.3. Der
deklarierte Bereich aller enthaltenen Apps muss zugleich den OpenDesk-Boden
Nextcloud 33 enthalten. `localbase` und bei mehreren Produkten `orgsuite`
sind mitgelieferte Infrastruktur.

## Verantwortlichkeiten vor der Inbetriebnahme

Vor dem ersten Import realer Personaldaten müssen benannt sein:

- technische Administration für Nextcloud, Datenbank, Backup und Wiederherstellung,
- fachliche Verantwortung für Organisationshierarchie, Gruppen und Freigaben,
- Ansprechperson für Datenschutz und betriebliche Mitbestimmung,
- Freigabeverantwortliche für Kalender, Assistenzplanung, Urlaub, Räume und Recruitment,
- Meldeweg und Reaktionszeit bei Störungen.

Die Suite ist eine Planungsanwendung, kein revisionssicheres Personalabrechnungs- oder Zeiterfassungssystem. Berechtigungen werden serverseitig aus Nextcloud-Konto, Gruppen, Organisationshierarchie und konfigurierten Peer-Freigaben ermittelt.

## Sicherungsumfang

Ein konsistenter Wiederherstellungspunkt umfasst immer gemeinsam:

- Datenbank-Dump der vollständigen Nextcloud-Datenbank,
- Nextcloud-`config/`,
- Nextcloud-`data/`,
- alle installierten AD-Fachapp-Verzeichnisse sowie `localbase` und gegebenenfalls `orgsuite` unter `custom_apps/`,
- das eingesetzte Releasebundle einschließlich `manifest.tsv`, `SHA256SUMS` und äußerer SHA-256-Datei,
- dokumentierte Nextcloud-, PHP-, Datenbank- und Webserverversion.

Die konkrete Sicherung richtet sich nach der Betriebsplattform. Während eines manuellen Dumps einer aktiven Instanz wird der Nextcloud-Wartungsmodus verwendet oder ein konsistenter Datenbank-Snapshot des Infrastrukturproviders erstellt. Ein Backup gilt erst nach einer protokollierten Testwiederherstellung als belastbar.

## Upgrade-Ablauf

1. Releasebundle und beide Prüfsummenebenen verifizieren.
2. Changelogs und `manifest.tsv` mit dem installierten Stand vergleichen.
3. Vollständigen Wiederherstellungspunkt erzeugen und Rücksicherung prüfen.
4. Wartungsfenster ankündigen und Nextcloud in den Wartungsmodus setzen.
5. Das neue Produktbundle mit seinem Installer einspielen oder beim vollständigen Suite-Bundle alle App-Verzeichnisse durch die neuen, unveränderten Archivwurzeln ersetzen; keine Versionsstände mischen.
6. Eigentümer und Dateirechte gemäß Nextcloud-Betrieb wiederherstellen.
7. `occ upgrade` ausführen, falls `occ status` ein Datenbankupgrade verlangt.
8. `occ status`, aktivierte Apps, Nextcloud-Log und die Abnahmesmokes prüfen.
9. Wartungsmodus erst nach erfolgreicher technischer Prüfung beenden.
10. Fachliche Kurzabnahme mit synthetischen Konten durchführen.

`--force` wird bei `occ app:enable` nicht verwendet. Ein Downgrade nach ausgeführten Migrationen erfolgt nicht durch alten Code auf neuer Datenbank, sondern durch Rücksicherung des vollständigen Wiederherstellungspunkts.

## Rückbau

Der Rückbauweg ist bewusst einfach und vollständig:

1. Instanz sperren beziehungsweise Wartungsmodus aktivieren.
2. Fehlerstand und Logs für die Nachanalyse sichern.
3. Datenbank, `config/`, `data/` und alle betroffenen App- und Infrastrukturverzeichnisse aus demselben Wiederherstellungspunkt zurückspielen.
4. Cache-/Opcode-Cache des Webservers leeren beziehungsweise PHP-FPM kontrolliert neu laden.
5. `occ status`, App-Liste und Nextcloud-Log prüfen.
6. Technische und fachliche Kurzabnahme wiederholen.

Ein einzelnes App-Verzeichnis wird nur dann isoliert zurückgerollt, wenn nachweislich keine Migration und kein app-übergreifender Vertragswechsel stattgefunden hat. `localbase` darf nicht deaktiviert oder entfernt werden, solange ein katalogisiertes AD-Fachprodukt aktiv ist. OrgSuite darf nur deaktiviert werden, wenn ihre AD-/BR-Navigation und ihr Adminadapter nicht mehr benötigt werden.

## Regelmäßige Betriebsprüfung

Mindestens nach Deployments und ansonsten nach betrieblichem Standard prüfen:

```bash
<RUNTIME-KONTEXT> <CLI-PHP> occ status
<RUNTIME-KONTEXT> <CLI-PHP> occ app:list --enabled
<RUNTIME-KONTEXT> <CLI-PHP> occ background:cron
<RUNTIME-KONTEXT> <CLI-PHP> occ config:system:get loglevel
```

Bei installiertem AD Kalender beziehungsweise LocalBase werden nach einem
Update zusätzlich die beiden periodischen Jobs gezielt geprüft. Die
Klassenfilter vermeiden, dass sie bei großen Instanzen außerhalb des
Standardlimits von `background-job:list` liegen:

```bash
<RUNTIME-KONTEXT> <CLI-PHP> occ background-job:list \
  --class='OCA\AdCalendar\BackgroundJob\ReconcileShiftCalendarsJob' \
  --output=json_pretty

<RUNTIME-KONTEXT> <CLI-PHP> occ background-job:list \
  --class='OCA\LocalBase\BackgroundJob\RefreshHolidayCalendarJob' \
  --output=json_pretty
```

Beide Befehle müssen bei aktiver App genau einen Eintrag liefern. Der Zeitstempel `1970-01-01` bedeutet unmittelbar nach der Registrierung lediglich, dass der Job noch nicht erstmals durch Cron gelaufen ist.

Zusätzlich kontrollieren:

- neue Fehler der Logger `orgsuite`, `adcalendar`, `adplaner`, `adurlaub`, `adroom`, `adrecruitment` und bei einer Entwicklungsinstallation `adbqplanung`,
- fehlgeschlagene Cron-/Background-Jobs,
- Datenbank-, Dateisystem- und Inode-Auslastung,
- Zertifikatsablauf und Erreichbarkeit der Nextcloud-Instanz,
- nicht erwartete Änderungen an App-Dateien,
- regelmäßige Wiederherstellungstests.

Personenbezogene Inhalte gehören nicht in Tickets, Chatverläufe oder ungeschützte Logauszüge. Diagnosematerial wird vor Weitergabe minimiert und pseudonymisiert.

## Störungsdiagnose

Bei einer Störung zuerst festhalten:

- Zeitpunkt, betroffene App, URL und Kontoart,
- erwartetes und beobachtetes Verhalten,
- HTTP-Status ohne Sitzungscookies oder CSRF-Token,
- Nextcloud-Request-ID und passende, bereinigte Logzeilen,
- installierte Appversionen und Git-Commits aus `manifest.tsv`,
- letzte Änderung an Gruppen, Hierarchie, Freigaben oder Appcode.

Bei unklaren Berechtigungsfehlern keine Rechte pauschal erweitern. Stattdessen Konto, direkte Gruppenmitgliedschaften, Organisationsdefinition und den konkreten serverseitigen Allow-/Deny-Fall prüfen.

## Release-Nachweis

Für jede Übergabe werden gemeinsam archiviert:

- Releasebundle und Prüfsummen,
- `manifest.tsv`,
- Ergebnis des Delivery-Gates,
- Ergebnis der HTTP-Smokes und gegebenenfalls Rechtematrizen,
- Coverage-Zusammenfassung,
- ausgefülltes Abnahmeprotokoll,
- dokumentierter Backup- und Rücksicherungstest,
- bekannte Einschränkungen und offene Risiken.
