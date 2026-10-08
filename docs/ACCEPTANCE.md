# Abnahmeprotokoll der Filzmann Full Suite

## Release und Umgebung

| Angabe | Wert |
| --- | --- |
| Releasebundle | |
| SHA-256 des Bundles | |
| Nextcloud-Version | |
| PHP-Version | |
| Datenbanktyp/-version | |
| Staging-URL | |
| Prüfdatum | |
| technisch geprüft durch | |
| fachlich geprüft durch | |

## Technische Pflichtprüfungen

- [ ] Äußere Bundle-Prüfsumme und alle Einträge in `SHA256SUMS` stimmen.
- [ ] Sieben App-Archive entsprechen `manifest.tsv` und wurden in dokumentierter Reihenfolge aktiviert.
- [ ] `occ status` meldet keinen ausstehenden Datenbank-Upgradebedarf.
- [ ] Bei aktivem Filzmann Kalender und LocalBase liefern die dokumentierten
  Klassenfilter für Kalenderabgleich und gemeinsamen Ferien-/Feiertagsabruf
  jeweils genau einen registrierten Hintergrundjob.
- [ ] Delivery-Gate einschließlich DDEV-/Zielserverstatus ist erfolgreich.
- [ ] Authentifizierte HTTP-Smokes sind erfolgreich.
- [ ] Rechtematrizen sind nach Änderungen an Gruppen, Hierarchie oder Rechten erfolgreich.
- [ ] Nextcloud-Log enthält nach den Prüfungen keine neuen unbehandelten Appfehler.
- [ ] Backup und vollständige Rücksicherung wurden protokolliert getestet.

## Fachliche Pflichtprüfungen

- [ ] Suite-Quermenü bleibt in Kalender, Assistenzplanung, Urlaub, Räumen und Filzmann Recruitment erreichbar.
- [ ] Bei einer gesonderten BQ-Entwicklungsinstallation ist der BQ-Planer im
  Suite-Quermenü und allein über seinen Standalone-Einstieg erreichbar; diese
  Prüfung ist kein Bundle- oder Release-Nachweis.
- [ ] Normale Konten sehen ausschließlich eigene, gemeinsame oder organisatorisch unterstellte Personen.
- [ ] Direkte API-Aufrufe auf nicht erlaubte Personen oder Adminfunktionen werden abgewiesen.
- [ ] Eigene Dienste und Termine können bearbeitet werden; Peerrechte bleiben auf freigegebene Gruppen und Bürobereiche begrenzt.
- [ ] Standarddienste erscheinen, gelöschte materialisierte Dienste bleiben gelöscht.
- [ ] Dienste, Termine, Sperrtermine, Urlaub und Meetinglücken greifen ohne widersprüchliche Buchungen ineinander.
- [ ] Genehmigter Urlaub blockiert; geplanter Urlaub warnt; Überlappungen derselben Person werden verhindert.
- [ ] Assistenzteams und Organisationsteams sind getrennt auswählbar und gemäß Hierarchie sichtbar.
- [ ] Raumbuchungen benötigen Titel und Zweck; Überschneidungen sowie ungültige Zeitraster werden verhindert.
- [ ] Stellen, Personen und Bewerbungen bleiben getrennt; kontrollierte Statuswechsel und abgeschlossene Interviews sind nachvollziehbar und unveränderlich.
- [ ] Organisationsweite Einstellungen sind nur im Nextcloud-Adminbereich sichtbar; App-Einstellungen betreffen nur das aktuelle Konto.
- [ ] Vertikales App-Scrolling, horizontaler Tabellenoverflow, Tastaturfokus und Dialogbedienung funktionieren.

## Datenschutz und Betrieb

- [ ] Zweck, Rechtsgrundlage, Aufbewahrung und Löschprozess für Personaldaten sind intern festgelegt.
- [ ] Administrations- und Supportverantwortung sowie Störungsmeldeweg sind benannt.
- [ ] Produktive Gruppen- und Organisationskonfiguration wurde von der fachlichen Verantwortung freigegeben.
- [ ] Bei LDAP-/Univention-Betrieb sind Gruppenfilter, stabile interne Nextcloud-Benutzer-IDs sowie typische Allow-/Deny-Fälle gemäß LDAP-Betriebsvertrag geprüft.
- [ ] Demo-Packs wurden nur bewusst im Adminbereich gestartet; fremde Konten und read-only LDAP-Gruppen blieben unverändert.
- [ ] Quellcode-/Lizenzbereitstellung und eingesetzte Drittkomponenten sind dokumentiert.

## Abweichungen und Entscheidung

Bekannte Abweichungen, Auflagen und Fristen:



- [ ] für Produktion freigegeben
- [ ] nur für weiteres Staging freigegeben
- [ ] nicht freigegeben

| Rolle | Name | Datum | Bestätigung |
| --- | --- | --- | --- |
| technische Verantwortung | | | |
| fachliche Verantwortung | | | |
| Datenschutz/Mitbestimmung | | | |
