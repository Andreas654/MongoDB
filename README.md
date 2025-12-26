```markdown
# MongoDB (Devcontainer / Codespaces)

Dieses Repository enthält eine Devcontainer- und Docker-Compose-Konfiguration, damit in einem Codespace / devcontainer
automatisch ein MongoDB-Server gestartet wird und `mongosh` in der Workspace-Shell verfügbar ist.

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
- Öffne ein Terminal im Workspace-Container:
   - mongosh startet nun automatisch beim Öffnen eines interaktiven Terminals (wartet bis MongoDB bereit ist).
   - Falls du das nicht möchtest, setze vor dem Terminal-Start die Variable `DISABLE_AUTO_MONGOSH=1`.
   - Manuell starten geht weiterhin: mongosh --host mongo -u $MONGO_INITDB_ROOT_USERNAME -p $MONGO_INITDB_ROOT_PASSWORD --authenticationDatabase admin

Troubleshooting: Fehler beim Start des Workspace
Fehlermeldung (Beispiel):
  Error: Command failed: docker compose -f /.../docker-compose.yml -f /.../docker-compose.codespaces.yml --profile * config


- Committe keine `.env` mit Passwörtern. Verwende `.env.example` als Vorlage.
- Ersetze Standardpassworte (z. B. `example`) durch sichere Werte.
```
