# Produktarchitektur der AD-Suite

Diese Datei beschreibt den geltenden Produkt- und Integrationsvertrag. Dieses
Delivery-Repository führt keine aktive systemweite Roadmap; sie liegt im
Parent-Workspace unter `docs/zukunftsplan.md`. Der aktuelle lokale
Kalenderkontext-/Feiertagsumbau ist bis zu getrennten Commits, Delivery-Gate
und Staging-Abnahme ein Arbeitsstand und kein veröffentlichter Release.

## Produkte und Infrastruktur

Verkaufbare Fachprodukte sind AD Kalender (`adcalendar`), Assistenzplanung
(`adplaner`), AD Urlaub (`adurlaub`), AD Raumplaner (`adroom`) und AD
Recruitment (`adrecruitment`). LocalBase und OrgSuite sind mitgelieferte
Infrastruktur und keine eigenständigen Fachprodukte.

Der BQ-Planer (`adbqplanung`) ist als sechstes, navigierbares
Entwicklungsprodukt katalogisiert. Seine `fullSuiteBundle`- und
`productBundle`-Flags bleiben bis zur dokumentierten Release-Reife `false`;
damit verändert er die aktuellen Lieferartefakte nicht.

Jedes Produktbundle enthält LocalBase, OrgSuite und genau ein Fachprodukt. Bei
genau einem aktiven Fachprodukt bleibt OrgSuite deaktiviert; ab zwei
Fachprodukten aktiviert der geprüfte Installer OrgSuite. Das vollständige
Suite-Bundle enthält alle sieben derzeit freigegebenen Apps. AD Recruitment besitzt zusätzlich ein
eigenes Produktpaket und wird nicht in die Pakete anderer Fachprodukte gelegt.

Der versionierte Produktkatalog in LocalBase ist die kanonische Quelle für
stabile Produkt-ID, Produkttyp, Reihenfolge, technische Einstiegsroute,
Standalone-, Menü- und Bundle-Zugehörigkeit. Sichtbare Labels werden im
Übersetzungsbereich der jeweiligen App aufgelöst. Navigation erteilt keine
Rechte und die Aufnahme in den Katalog ersetzt keine serverseitige
Zielberechtigung.

Fachprodukte bleiben ohne optionale Provider nutzbar. Fehlende Apps führen zu
einem dokumentierten manuellen oder reduzierten Standalone-Weg und nicht zu
einem Installationsfehler.

## Navigation, Administration und Rechte

OrgSuite stellt die gemeinsamen AD-/BR-Einstiege und ab zwei AD-Produkten den
Adminadapter für app-übergreifende Organisationseinstellungen bereit. Bei
Einzelinstallation stellt das Fachprodukt den Einstieg und den Adapter.
Persistenz, Validierung und geschützte Admin-API der gemeinsamen
Organisationsdefinition liegen in LocalBase.

Navigation und Capability-Verfügbarkeit erweitern keine fachlichen Rechte.
Jede Fachapp erzwingt Lesen, Schreiben, Administration und konkrete
Zielobjekte serverseitig.

## Gemeinsame Verträge

Rollen, Bereiche, Assistenzteams, Hierarchie, Reihenfolge und Peergrenzen
stammen aus der konfigurierbaren `AdOrganizationDefinition`. Fachapps führen
keine parallelen Rollenregister und greifen nicht direkt auf Tabellen,
Controller oder Assets anderer Fachapps zu.

Optionale Integrationen verwenden kleine read-only Events oder
Capability-Verträge in LocalBase. AD Urlaub ist die schreibende Urlaubsquelle;
AD Kalender konsumiert Abwesenheiten read-only. Fachliche Kalenderdaten
bleiben in der jeweils zuständigen Fachapp.

## Kalenderkontext und Jahreskalender

Im aktuellen lokalen Arbeitsstand führt LocalBase Land, ISO-3166-2-Region und
fachliche IANA-Zeitzone organisationsweit. `DE`, `DE-BE` und
`Europe/Berlin` bleiben Bestandsdefaults. Persönliche Nextcloud-Zeitzonen
beeinflussen nur individuelle Anzeigen.

LocalBase liefert außerdem Schulferien und gesetzliche Feiertage als
regionsgebundenen read-only Jahresvertrag. Der OpenHolidays-Provider wird
validiert und zwischengespeichert; vorhandene Daten bleiben bei Ausfällen mit
erkennbarer Aktualität verfügbar. AD Urlaub, AD Kalender und AD Raumplaner
entscheiden jeweils selbst über Darstellung und fachliche Wirkung.

## Delivery

Produktarchive enthalten keine Tests, Git-Metadaten, `AGENTS.md`, Symlinks,
Secrets oder fremde Fachprodukte. Manifest, innere und äußere SHA-256-Summen
und Archivwurzel sind Teil des Liefervertrags. Ein grünes lokales Gate ersetzt
weder frische Staging-Installation, Backup/Rückbau, Datenschutz- und
Mitbestimmungsprüfung noch fachliche Freigabe.
