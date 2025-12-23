```markdown
# MongoDB (Devcontainer / Codespaces)

Dieses Repository enthält eine Devcontainer- und Docker-Compose-Konfiguration, damit in einem Codespace / devcontainer automatisch ein MongoDB-Server gestartet wird und `mongosh` in der Workspace-Shell verfügbar ist.

Wesentliche Dateien
- `docker-compose.yml` — startet die Services `mongo` und `workspace`.
- `.devcontainer/devcontainer.json` — Devcontainer-Konfiguration (verwendet den `workspace`-Service).
- `.devcontainer/Dockerfile` — installiert `mongosh` im Workspace-Container.
- `.env.example` — Beispiel für Umgebungsvariablen (kopieren zu `.env` und anpassen; `.env` nicht ins VCS committen).

Schnellstart (lokal)
1. Kopiere `.env.example` zu `.env` und passe Benutzer/Passwort an:
   - cp .env.example .env
   - Bearbeite `.env` und passe `MONGO_INITDB_ROOT_USERNAME` und `MONGO_INITDB_ROOT_PASSWORD` an.

2. Starte die Services:
   - docker compose up -d

3. Prüfe Logs / Status:
   - docker compose ps
   - docker compose logs -f mongo

4. MongoDB-Shell öffnen:
   - docker compose exec mongo mongosh -u <user> -p <password> --authenticationDatabase admin
   oder (aus dem workspace-container):
   - docker compose exec workspace mongosh --host mongo -u <user> -p <password> --authenticationDatabase admin

Schnellstart (Codespaces / Devcontainer)
- Öffne das Repository in GitHub Codespaces oder mit VS Code Remote - Containers -> "Reopen in Container".
- Codespaces / devcontainer baut automatisch die im `docker-compose.yml` definierten Services.
- Öffne ein Terminal im Workspace-Container und führe aus:
  - mongosh --host mongo -u $MONGO_INITDB_ROOT_USERNAME -p $MONGO_INITDB_ROOT_PASSWORD --authenticationDatabase admin

Troubleshooting: Fehler beim Start des Workspace
Fehlermeldung (Beispiel):
  Error: Command failed: docker compose -f /.../docker-compose.yml -f /.../docker-compose.codespaces.yml --profile * config

Was du sofort prüfen kannst:
1. YAML-Validierung & Compose-Konfiguration lokal testen:
   - docker compose -f docker-compose.yml config

2. Version / Compose-CLI:
   - docker compose version

3. Häufige Ursachen und Lösungen
   - Ungültige `depends_on`-Syntax: Verwende einfache Liste `depends_on: [mongo]`.
   - Fehlerhafte Einrückungen: Verwende zwei Leerzeichen pro Level, keine Tabs.
   - `profiles` / `--profile`-Probleme: Falls ein externes `--profile` übergeben wird, teste `docker compose config` lokal ohne Profile.

4. Wenn `docker compose ... config` selbst den oben genannten Fehler zurückgibt:
   - Führe denselben Befehl im Codespace-Terminal aus, dort wird meist eine detailliertere Fehlermeldung angezeigt.
   - Kopiere mir die Ausgabe von:
     docker compose -f /var/lib/docker/codespacemount/workspace/MongoDB/docker-compose.yml -f /var/lib/docker/codespacemount/.persistedshare/docker-compose.codespaces.yml --profile '*' config
     (oder die vereinfachte: `docker compose -f docker-compose.yml config`)
   - Ich schaue mir die Ausgabe an und korrigiere die Stelle in der YAML.

Sicherheit
- Committe keine `.env` mit Passwörtern. Verwende `.env.example` als Vorlage.
- Ersetze Standardpassworte (z. B. `example`) durch sichere Werte.
```
