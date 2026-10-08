# LDAP- und Univention-Betriebsvertrag

Die Filzmann Full Suite ist mit LDAP-Konten und -Gruppen kompatibel, weil sie ausschließlich die Benutzer-, Sitzungs- und Gruppen-APIs von Nextcloud verwendet. Die Fachapps greifen weder direkt auf LDAP noch auf Univention-Verzeichnisdaten zu. Damit gelten dieselben fachlichen Rechte unabhängig davon, ob ein Konto aus Univention/LDAP oder aus dem lokalen Nextcloud-Backend stammt.

## Verbindliche Voraussetzungen

Vor der fachlichen Abnahme müssen Administrator*innen prüfen:

1. Alle in der FLZ-Organisation konfigurierten Gruppen-IDs sind für Nextcloud sichtbar. Anzeigenamen allein reichen nicht; maßgeblich ist die interne Nextcloud-Gruppen-ID.
2. Die internen Nextcloud-Benutzer-IDs bleiben stabil. Die Filzmann Full Suite speichert diese IDs an Diensten, Terminen, Urlauben, Plänen und Buchungen. LDAP-UUID- oder Internal-Username-Einstellungen dürfen nach Produktivnahme nicht ungeprüft geändert und LDAP-Mappings nicht gelöscht werden.
3. Kritische Rollen verwenden direkte, in Nextcloud erkennbare Mitgliedschaften. Verschachtelte oder dynamische LDAP-Gruppen werden erst nach einem eigenen Allow-/Deny-Test freigegeben.
4. Ein- und Austritte, Umbenennungen und deaktivierte Verzeichniskonten besitzen einen abgestimmten Lifecycle. Ein LDAP-Konto darf erst entfernt oder neu zugeordnet werden, nachdem fachliche Datensätze und Aufbewahrungspflichten geprüft wurden.
5. Änderungen an read-only LDAP-Gruppen erfolgen in Univention. Alternativ dürfen bewusst angelegte lokale Nextcloud-Berechtigungsgruppen LDAP-Konten enthalten, wenn dies organisatorisch freigegeben ist.

Die Organisationsadministration zeigt für jede konfigurierte Gruppe Existenz, Backend und Schreibbarkeit an. Eine read-only LDAP-Gruppe ist für produktive Rechte kein Fehler. Sie ist nur für Vorgänge ungeeignet, die Gruppenmitgliedschaften verändern sollen.

## Demo-Packs

Demo-Packs werden niemals automatisch installiert. Sie werden ausschließlich im Adminbereich der jeweiligen Fachapp nach ausdrücklicher Bestätigung gestartet und verwenden keine WordPress-Bestandsdaten.

Für synthetische Demokonten gelten zusätzliche Grenzen:

- Ein bereits vorhandenes fremdes oder LDAP-verwaltetes Konto wird niemals unter einer Demo-UID übernommen oder verändert.
- Ein Demo-Pack prüft alle bestehenden Zielgruppen vor der ersten Änderung.
- Eine schreibgeschützte LDAP-Gruppe bricht die Installation vor der ersten Änderung ab.
- Neu erzeugte Demokonten werden mit App-Eigentümerschaft registriert und nur vom selben Demo-Pack idempotent wiederverwendet.
- Demo-Packs verändern keine realen Mitgliedschaften und wählen keine zufälligen realen Gruppenmitglieder als Beispieldatensatz aus.

## Abnahme mit LDAP-Konten

Mindestens folgende Fälle werden mit synthetischen, über denselben Univention-/LDAP-Weg wie später produktiv bereitgestellten Konten geprüft:

- Mitglied einer eigenen Fachgruppe darf die vorgesehenen eigenen Daten lesen und schreiben.
- Direkte Kolleg*innen erhalten nur explizit freigeschaltete Peerrechte und nur im zulässigen Büro-/Fachbereich.
- Vorgesetzte sehen und bearbeiten ausschließlich organisatorisch unterstellte Gruppen.
- Unterstellte Personen können keine Daten einer übergeordneten Gruppe verändern.
- Ein fachfremdes Konto wird serverseitig abgewiesen, auch bei direktem API-Aufruf.
- Ein Gruppenwechsel wirkt nach der LDAP-Synchronisation wie erwartet, ohne eine neue interne Nextcloud-Benutzer-ID zu erzeugen.
- Read-only LDAP-Gruppen funktionieren lesend für Berechtigungen und werden durch Demo-Packs nicht verändert.

## Weiterführende Herstellerdokumentation

- [Nextcloud: LDAP user and group backend](https://docs.nextcloud.com/server/stable/admin_manual/configuration_user/user_auth_ldap.html)
- [Nextcloud: LDAP API und interne Benutzernamen](https://docs.nextcloud.com/server/latest/admin_manual/configuration_user/user_auth_ldap_api.html)
- [Univention: Nextcloud konfigurieren](https://docs.software-univention.de/packaged-integration-nextcloud/latest/en/configure-nextcloud.html)
- [Univention: Gruppenverwaltung](https://docs.software-univention.de/manual/5.0/en/groups.html)
