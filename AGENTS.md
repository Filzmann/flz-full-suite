# AGENTS.md – AD Suite

## Zweck

Dieses eigenständige Repository ist die öffentliche Produkt- und Releaseübersicht der AD-Suite für Nextcloud. Deploybarer App-Code verbleibt in den sieben getrennten App-Repositories.

Enthalten sind ausschließlich:

- Produktübersicht und Links zu den App-Repositories,
- Installations-, Betriebs-, Rückbau- und Abnahmeunterlagen,
- öffentlich geeignete Releasehinweise und Prüfsummen.

Releasearchive werden als GitHub-Release-Assets veröffentlicht und nicht dauerhaft im Git-Tree versioniert.

## Repository-Grenzen

- Kein App-Code und keine Nextcloud-Migrationen in dieses Repository verschieben.
- Keine internen DDEV-Pfade, Zugangsdaten, realen Personen-, Team- oder Falldaten veröffentlichen.
- Technische Beispiele verwenden ausschließlich neutrale Bezeichnungen wie Team A, Team B und Team C.
- Quellcodeänderungen werden im jeweils zuständigen App-Repository vorgenommen.

## Veröffentlichung

- Vor jedem Release das Delivery-Gate im lokalen Parent-Workspace erfolgreich ausführen.
- Versionen, Commit-IDs und SHA-256-Prüfsummen müssen mit dem erzeugten Manifest übereinstimmen.
- Releasekandidaten klar als nicht produktionsfreigegeben kennzeichnen.
- Installation auf einem frischen Staging-System, Rückbau, Datenschutzprüfung und fachliche Abnahme bleiben vor Production verpflichtend.

## Git

- Vor Commits `git status --short`, `git diff --stat` und `git diff --name-only` prüfen.
- Dateien gezielt stagen; niemals `git add .`.
- Push und GitHub-Releases nur nach ausdrücklicher Freigabe durch Simon. Eine Veröffentlichungsfreigabe gilt ausschließlich für den konkret benannten Releasekontext und die konkret benannte Version; diese Datei dokumentiert keine zeitlich unbegrenzte oder aktuell offene Erstveröffentlichungsfreigabe.

## Parent-Governance-Vertrag: 1

- Die für dieses Subrepository anwendbaren Regeln des Parent-Workspaces sind
  verbindlich. Dazu gehören insbesondere app-übergreifende ADRs und
  öffentliche Verträge, Repositorygrenzen sowie Workspace-, Delivery- und
  Release-Gates.
- Diese lokale `AGENTS.md` und die lokalen Skills bleiben die vollständige,
  ohne Parent-Checkout arbeitsfähige Repository-Steuerung. Die anwendbaren
  Parent-Regeln werden dafür hier oder in den lokalen Skills mitgeführt.
- Repository-lokale Regeln dürfen Parent-Verträge konkretisieren und verschärfen,
  aber nicht abschwächen oder umgehen.
- Bei einem Widerspruch gilt bis zur Klärung die strengere Regel. Die Arbeit
  stoppt, bis die kanonische Quelle bestimmt, die Regelprojektionen
  synchronisiert und eine erforderliche Entscheidung dokumentiert ist.
- Ist der Parent-Workspace nicht verfügbar, bleibt die lokale Steuerung
  wirksam. Vor Cross-App-, Release- oder Delivery-Arbeit muss ein vermuteter
  neuerer Parent-Stand oder eine Regelungslücke zuerst gegen den Parent
  geprüft werden.
