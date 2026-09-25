# UD14 — Consegna LAB autonomo

- branch: `feature/ud14-ci-v2`
- modifica app: aggiornata `APP_VERSION` in `server.py` da `ci-v1` a `ci-v2`
- test fallito: `test_health_version`
- causa:  il test si aspettava ancora `ci-v1`, mentre l'applicazione restituiva `ci-v2`
- modifica test: aggiornato il valore atteso in `test_backend.py` da `ci-v1` a `ci-v2`
- test finale: 3 test eseguiti con esito ok
- PR: creata da `feature/ud14-ci-v2` verso `main`
- merge: completato
- run CI: - run CI: avviata automaticamente dal trigger su `main`; Stage Test e Stage BuildPush completati con successo
- nuovo tag ACR: 10
- cleanup eseguito: NO
