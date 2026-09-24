# Amministrazione

L'amministrazione del catalogo avviene tramite `admin.html`.

## Credenziali

- Utente: `hq`
- Password: vedi `credenziali.txt` (memo locale — **non pubblicare** questo file).

L'autenticazione viene eseguita contro `deploy/.htpasswd` (formato `$6$` = SHA-512 crypt),
un formato compatibile con `htpasswd` di Apache per il deploy.

## Workflow di aggiornamento dei dati

1. Carica i **file** in `dataset/` tramite FTP sul server.
2. Apri `admin.html` e aggiorna i **metadati** (titolo, ente, formati, percorsi file…).
   Nell'editor "File del dataset" aggiungi una riga per ogni formato (es. JSON e CSV dello
   stesso dataset).
3. Salva: in locale la modifica viene scritta in `config/datasets.json` dal `server.py`;
   sul deploy il salvataggio online richiede **WebDAV** abilitato, altrimenti usa il
   pulsante **"Esporta catalogo"** e ricarica il file `datasets.json` via FTP in `config/`.

> Eliminare una voce dal catalogo NON cancella il file in `dataset/`.

## Note di sicurezza

- `server.py` è solo per sviluppo locale (`python -m http.server` non consentirebbe il
  salvataggio del catalogo).
- Le password in `.htpasswd` sono hash SHA-512 crypt (non in chiaro).
- Il progetto è intenzionalmente senza database: i file JSON sono lo storage.