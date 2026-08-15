# AD Suite für Nextcloud

Die AD Suite führt sechs eigenständige Fachprodukte für Dienst-, Assistenz-,
Urlaubs-, Raum-, Bewerbungs- und Basisqualifizierungsprozesse unter einer
gemeinsamen Navigation und Organisationskonfiguration. Der aktuelle
Release-Candidate bündelt weiterhin die fünf bereits lieferbaren Produkte
mit LocalBase und OrgSuite, insgesamt sieben Apps.

> Status: Release Candidate für ein kontrolliertes Staging auf Nextcloud 34 mit PHP ab 8.3. Vor einem produktiven Einsatz sind Neuinstallationstest, Datenschutz- und Mitbestimmungsprüfung, Sicherheitsreview und fachliche Abnahme erforderlich.

## Bestandteile

| App | Aufgabe | Quellcode |
| --- | --- | --- |
| LocalBase | Gemeinsame Organisations-, Rechte-, Kalender- und UI-Verträge | [nextcloud-localbase](https://github.com/Filzmann/nextcloud-localbase) |
| OrgSuite | Gemeinsame AD-/BR-Navigation und app-übergreifende Administration | [nextcloud-orgsuite](https://github.com/Filzmann/nextcloud-orgsuite) |
| AD Kalender | Dienste, Termine, Sperrtermine, Personensuche und Meetinglücken | [nextcloud-adcalendar](https://github.com/Filzmann/nextcloud-adcalendar) |
| Assistenzplanung | Monatliche Wunschdienstplanung für dynamische Assistenzteams | [nextcloud-adplaner](https://github.com/Filzmann/nextcloud-adplaner) |
| AD Urlaub | Geplante und genehmigte Urlaube mit Rechte- und Konfliktprüfung | [nextcloud-adurlaub](https://github.com/Filzmann/nextcloud-adurlaub) |
| AD Raumplaner | Zeitlich ausgerichtete, kollisionsfreie Raumbuchungen | [nextcloud-adroom](https://github.com/Filzmann/nextcloud-adroom) |
| AD Recruitment (`adrecruitment`) | Stellen, Personen, Bewerbungen und versionierte Interviews | [nextcloud-recruitment](https://github.com/Filzmann/nextcloud-recruitment) |
| AD BQ-Planer (`adbqplanung`) | Basisqualifizierungen, Curricula, Dozentinnen und Praxisreflexionen | noch nicht öffentlich veröffentlicht |

Die fünf freigegebenen Fachapps sind einzeln verkauf-, installier- und nutzbar. Jedes
Produktbundle bringt LocalBase und OrgSuite als kompatible Infrastruktur mit;
ab zwei Fachprodukten wird OrgSuite für gemeinsame Navigation und
Administration aktiviert. Fehlende Integrationspartner werden nicht als
Fehler behandelt: Die jeweilige Direktfunktion entfällt oder bleibt als
manueller Fachweg verfügbar.

Der versionierte LocalBase-Produktkatalog trennt Menüzugehörigkeit,
Standalone-Fähigkeit und Bundle-Zugehörigkeit. AD Recruitment gehört zum
gemeinsamen AD-Menü, zum vollständigen AD-Suite-Archiv und erhält ein eigenes
Produktpaket. Es wird nicht in Produktpakete anderer Fachapps aufgenommen.
Der BQ-Planer ist bereits Menü- und Standalone-Produkt, seine Full-Suite- und
Einzelbundle-Flags bleiben bis zur Release-Reife deaktiviert.

Navigation erteilt keine Rechte; schreibende und lesende Zugriffskontrollen werden serverseitig in den jeweiligen Apps durchgesetzt. LocalBase und OrgSuite sind mitgelieferte Infrastruktur, keine separat vermarkteten Fachprodukte.

## Installation und Abnahme

- [Suiteweite Roadmap](ROADMAP.md)
- [Produktarchitektur](docs/ARCHITECTURE.md)
- [Aktiver Ausführungsplan](docs/IMPLEMENTATION-PLAN.md)
- [Maschinenlesbares Hardcoding-Inventar](docs/HARDCODING-INVENTORY.json)
- [Staging-Installation](docs/INSTALLATION.md)
- [LDAP- und Univention-Betriebsvertrag](docs/LDAP-UNIVENTION.md)
- [Betrieb und Rückbau](docs/OPERATIONS.md)
- [Abnahmeprotokoll](docs/ACCEPTANCE.md)
- [Delivery-Gate und Testabdeckung](docs/DELIVERY-GATE.md)

Die fünf Produktbundles `ad-product-<app-id>-<release>.tar.gz`, das vollständige Suite-Bundle, Versionsmanifeste und SHA-256-Prüfsummen werden gemeinsam im jeweiligen GitHub-Release bereitgestellt. Produktinstallationen verwenden das enthaltene `install.sh` und nicht einzelne rohe Fachapp-Archive.

## Lizenz und Leistungen

Der Quellcode der Apps steht unter der GNU Affero General Public License v3.0. Installation, Einführung, Anpassung, Schulung, Wartung und Support können unabhängig davon als nicht exklusive Dienstleistungen vereinbart werden.

Fehler und Funktionswünsche gehören in das jeweils betroffene App-Repository. Sicherheitsrelevante Hinweise bitte nicht öffentlich mit personenbezogenen Daten dokumentieren, sondern über die privaten Security-Advisories des betroffenen GitHub-Repositories melden.
