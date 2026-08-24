# AD-Suite auf einem Staging-Server installieren

Diese Anleitung gilt für den ersten internen Releasekandidaten der AD-Suite auf Nextcloud 34 mit PHP ab 8.3. Die bisher verifizierte Referenzumgebung verwendet MySQL/MariaDB.

## Liefermodelle

Verkaufbare Fachprodukte sind:

- `adcalendar` – AD Kalender,
- `adplaner` – Assistenzplanung,
- `adurlaub` – AD Urlaub,
- `adroom` – AD Raumplaner.
- `adrecruitment` – AD Recruitment.

`adbqplanung` – AD BQ-Planer – ist als Entwicklungsprodukt katalogisiert,
aber noch nicht Bestandteil eines Installations- oder Suite-Bundles.

Jedes Produktbundle enthält zusätzlich eine kompatible Version von `localbase` und `orgsuite`. Diese beiden Apps sind mitgelieferte technische Infrastruktur und keine separaten Fachprodukte. Die Fachapps funktionieren einzeln. Ab zwei aktivierten AD-Fachprodukten bündelt OrgSuite Navigation und Organisationsadministration.

Es gibt zwei Paketarten:

- `ad-product-<app-id>-RELEASE.tar.gz` für die Installation oder Aktualisierung genau eines Fachprodukts,
- `ad-suite-RELEASE.tar.gz` für eine vollständige Installation aller fünf Fachprodukte.

AD Recruitment wird im vollständigen AD-Suite-Archiv und als eigenes
Produktpaket ausgeliefert. Es wird nicht stillschweigend Bestandteil eines
anderen Fachproduktpakets. Der BQ-Planer wird in diesem Release noch nicht
ausgeliefert. Menü- und Bundle-Zugehörigkeit werden im
mitgelieferten `ad-product-catalog.json` getrennt ausgewiesen; Navigation
erteilt keine Rechte.

Für Einzelprodukte ist immer der mitgelieferte Installer zu verwenden. Nextcloud 34 installiert App-Abhängigkeiten aus `info.xml` nicht automatisch; das Produktbundle übernimmt deshalb Reihenfolge, Prüfsummen und Aktivierung der Infrastruktur.

## Server vorab prüfen

Vor jedem Befehl werden Nextcloud-Root, tatsächliches CLI-PHP, erforderliches
CLI-Memory-Limit und der für Installation beziehungsweise Upgrade vorgesehene
Runtime-/Domainbenutzer aus der realen Zielkonfiguration ermittelt. Die
Platzhalter `<NEXTCLOUD-ROOT>`, `<CLI-PHP>` und `<RUNTIME-KONTEXT>` werden
durch diese geprüften Werte ersetzt; `www-data`, System-`php` oder
`/var/www/nextcloud` sind keine allgemeinen Vorgaben.

```bash
cd <NEXTCLOUD-ROOT>
<RUNTIME-KONTEXT> <CLI-PHP> occ status
<RUNTIME-KONTEXT> <CLI-PHP> occ config:system:get dbtype
<RUNTIME-KONTEXT> <CLI-PHP> occ integrity:check-core
<CLI-PHP> -v
```

Webserver und CLI müssen dieselbe unterstützte PHP-Hauptversion verwenden. Das Verzeichnis `custom_apps/` muss existieren und für den vorgesehenen Deploymentprozess beschreibbar sein.

## Backup und Rückbau

Vor einer Installation auf einer bereits genutzten Instanz mindestens sichern:

- Nextcloud-Datenbank,
- `config/`,
- `data/`,
- vorhandenes `custom_apps/`,
- gegebenenfalls das Theme.

Nach ausgeführten App-Migrationen ist ein Downgrade durch bloßes Zurückkopieren alten App-Codes nicht sicher. Der Rückbau erfolgt durch Wiederherstellung des zusammengehörigen Datenbank-, Konfigurations-, Daten- und App-Backups.

## Einzelprodukt installieren

Produktbundle und äußere Prüfsumme gemeinsam übertragen. Im Beispiel wird `PRODUCT` durch `adcalendar`, `adplaner`, `adurlaub`, `adroom` oder `adrecruitment` und `RELEASE` durch die konkrete Releasebezeichnung ersetzt:

```bash
sha256sum --check ad-product-PRODUCT-RELEASE.tar.gz.sha256
tar -xzf ad-product-PRODUCT-RELEASE.tar.gz
cd ad-product-PRODUCT-RELEASE
sha256sum --check SHA256SUMS
<RUNTIME-KONTEXT> ./install.sh \
  --nextcloud-root <NEXTCLOUD-ROOT> \
  --bundle-dir "$PWD" \
  --product PRODUCT
```

Der Installer:

1. prüft alle inneren SHA-256-Summen und Archivwurzeln,
2. installiert beziehungsweise aktualisiert zuerst `localbase`, dann die Fachapp,
3. verhindert ein unbeabsichtigtes Downgrade auf eine gleiche oder ältere Appversion,
4. lässt OrgSuite bei genau einem aktiven AD-Produkt deaktiviert,
5. aktiviert OrgSuite automatisch, sobald mindestens zwei AD-Produkte aktiv sind,
6. führt `occ upgrade` für Aktualisierungen bereits aktiver Apps aus,
7. deaktiviert bei einem Fehler vor dem Datenbankupgrade neu aktivierte Apps wieder und stellt zuvor vorhandene Appverzeichnisse wieder her.

Der tatsächlich verwendete Deployment-/Runtimekontext benötigt die
erforderlichen Schreibrechte auf dem vorgesehenen `custom_apps`-Pfad.
Vorhandene Appdaten in der Datenbank werden nicht gelöscht. App-Migrationen
laufen beim `occ app:enable` beziehungsweise `occ upgrade`; deshalb bleibt das
vollständige Backup auch beim Produktinstaller Pflicht. Scheitert
`occ upgrade`, setzt der Installer den Appcode bewusst nicht automatisch
zurück, weil bereits ausgeführte Datenbankmigrationen sonst nicht mehr zum
alten Code passen könnten. In diesem Fall wird der vollständige
Wiederherstellungspunkt eingespielt.

OrgSuite wird nicht automatisch deaktiviert, wenn später ein Produkt manuell deaktiviert wird. So werden vorhandene BR-Navigation und bewusst konfigurierte Suite-Nutzung nicht überraschend verändert.

## Vollständige Suite installieren

Das Suite-Bundle und die danebenliegende Prüfsumme gemeinsam übertragen. Im Beispiel wird `RELEASE` vorher durch die konkrete Releasebezeichnung wie `nc34-rc2` ersetzt:

```bash
sha256sum --check ad-suite-RELEASE.tar.gz.sha256
tar -xzf ad-suite-RELEASE.tar.gz
cd ad-suite-RELEASE
sha256sum --check SHA256SUMS
<RUNTIME-KONTEXT> ./install.sh \
  --nextcloud-root <NEXTCLOUD-ROOT> \
  --bundle-dir "$PWD" \
  --product suite
```

`manifest.tsv` dokumentiert pro App Version, Git-Commit, SHA-256 und Signaturstatus.

Der Suite-Installer nutzt dieselben Prüfungen, Backups und Rückbaugrenzen wie der Einzelproduktinstaller. Er installiert alle fünf Fachprodukte, LocalBase und OrgSuite in der erforderlichen Reihenfolge und aktiviert anschließend die vollständige Suite. Die folgenden manuellen Schritte dienen nur als dokumentierter Ausweichweg, falls der Installer vor Beginn der Aktivierung nicht ausgeführt werden kann.

### Manueller Ausweichweg: Apps entpacken

```bash
<DEPLOY-KONTEXT> tar -xzf localbase-*.tar.gz -C <CUSTOM-APPS>/
<DEPLOY-KONTEXT> tar -xzf orgsuite-*.tar.gz -C <CUSTOM-APPS>/
<DEPLOY-KONTEXT> tar -xzf adcalendar-*.tar.gz -C <CUSTOM-APPS>/
<DEPLOY-KONTEXT> tar -xzf adplaner-*.tar.gz -C <CUSTOM-APPS>/
<DEPLOY-KONTEXT> tar -xzf adurlaub-*.tar.gz -C <CUSTOM-APPS>/
<DEPLOY-KONTEXT> tar -xzf adroom-*.tar.gz -C <CUSTOM-APPS>/
<DEPLOY-KONTEXT> tar -xzf adrecruitment-*.tar.gz -C <CUSTOM-APPS>/
```

Jedes Archiv enthält genau den zur App-ID passenden Wurzelordner. Keine Ordner umbenennen.
Eigentümer, Gruppe und Modi werden danach mit der kleinsten zur realen
Runtime-/Static-Webserver-Konfiguration passenden Änderung gesetzt; keine
pauschale rekursive Beispielberechtigung übernehmen.

### Manueller Ausweichweg: Apps aktivieren

Auf einem leeren Staging-System können die Apps direkt in Abhängigkeitsreihenfolge aktiviert werden. Auf einer bereits benutzten Instanz empfiehlt sich für den Installationszeitraum der Wartungsmodus.

```bash
cd <NEXTCLOUD-ROOT>
<RUNTIME-KONTEXT> <CLI-PHP> occ app:enable localbase
<RUNTIME-KONTEXT> <CLI-PHP> occ app:enable orgsuite
<RUNTIME-KONTEXT> <CLI-PHP> occ app:enable adcalendar
<RUNTIME-KONTEXT> <CLI-PHP> occ app:enable adplaner
<RUNTIME-KONTEXT> <CLI-PHP> occ app:enable adurlaub
<RUNTIME-KONTEXT> <CLI-PHP> occ app:enable adroom
<RUNTIME-KONTEXT> <CLI-PHP> occ app:enable adrecruitment
<RUNTIME-KONTEXT> <CLI-PHP> occ status
<RUNTIME-KONTEXT> <CLI-PHP> occ app:list --enabled
```

`--force` darf nicht verwendet werden. Beim Aktivieren führt Nextcloud die
noch ausstehenden App-Migrationen aus. Meldet `occ status` danach
`needsDbUpgrade: true`, wird im Wartungsfenster
`<RUNTIME-KONTEXT> <CLI-PHP> occ upgrade` ausgeführt.

## Signaturen

Interne, unsignierte RC-Archive sind auf einem privaten Staging-Server installierbar. `occ integrity:check-app <app-id>` meldet dann, dass keine Signatur vorhanden ist und überspringt die Dateiintegritätsprüfung.

Sobald offizielle app-spezifische Zertifikate vorliegen, kann der Release-Builder mit `SIGNING_KEY_DIR` und `NEXTCLOUD_ROOT` signierte Archive erzeugen. Private Schlüssel dürfen niemals im Workspace, Releasearchiv oder auf dem Webserver abgelegt werden.

## Organisationskonfiguration

Bei einem einzelnen AD-Fachprodukt erscheinen die organisationsweiten Einstellungen im Nextcloud-Adminabschnitt dieses Produkts. Ab zwei aktivierten AD-Fachprodukten erscheinen sie im Adminabschnitt der OrgSuite. Dort prüfen:

- Rollen- und Gruppen-IDs,
- Bereiche Nordost, West und Süd,
- Leitungshierarchie,
- Kalender-Peerrechte,
- Urlaubs-Peerrechte.

Die ausschließlich appbezogenen Raumstammdaten bleiben unabhängig davon im eigenen Nextcloud-Adminabschnitt `AD Raumplaner`.

Bei Univention-/LDAP-Betrieb ist zusätzlich der [LDAP- und Univention-Betriebsvertrag](LDAP-UNIVENTION.md) abzuarbeiten. Insbesondere müssen interne Nextcloud-Benutzer-IDs stabil bleiben und alle konfigurierten Gruppen-IDs in Nextcloud sichtbar sein.

Fehlende Fachapps sind ein unterstützter Standalone-Zustand: Ohne AD Urlaub bleiben manuelle Sperrtermine im Kalender möglich; ohne AD Kalender bleibt Urlaubsplanung möglich, jedoch ohne automatische Dienstkonfliktprüfung; Raumbuchungen, Assistenzplanung und AD Recruitment bleiben ohne die jeweils anderen Produkte eigenständig nutzbar.

Demo-Packs werden nie automatisch ausgeführt und importieren keine WordPress-Bestandsdaten. Sie dürfen ausschließlich nach bewusster Bestätigung im Adminbereich der jeweiligen Fachapp installiert werden. Auf einem realitätsnahen LDAP-Staging müssen dafür synthetische Konten und schreibbare Demogruppen verwendet werden; read-only LDAP-Gruppen werden nicht verändert.

## Abnahmekriterien

- Bei einer Fachapp ist ihr eigener Nextcloud-Einstieg sichtbar; ab zwei Fachapps bleibt das Suite-Menü in allen Fachapps sichtbar.
- Organisationsadministration erscheint bei einer Fachapp dort und ab zwei Fachapps ausschließlich in OrgSuite.
- Normale Konten sehen nur eigene, gemeinsame oder unterstellte Sichten.
- Direkte API-Aufrufe auf verbotene Ziele werden serverseitig abgewiesen.
- Schreibzugriffe ohne CSRF-Token ergeben HTTP 412.
- Kalender, Standarddienste, Urlaub und Meetinglücken greifen korrekt ineinander.
- Raumüberschneidungen werden mit HTTP 409 verhindert.
- Vertikales App-Scrolling und horizontaler Tabellenoverflow funktionieren.
- Nextcloud-Log enthält nach den Abnahmeläufen keine neuen Appfehler.

Erst nach erfolgreicher Abnahme werden reale Personaldaten verwendet. Ein Import von WordPress-Bestandsdaten ist nicht Bestandteil des Produkts.
