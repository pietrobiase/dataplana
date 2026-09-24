# Avvio rapido

Come far girare DataPlana in locale, sul tuo computer, per sviluppare o provare il portale.

## Avvio del server locale

Il progetto include un piccolo server Python (`server.py`) che non è il deploy reale ma
serve solo per lo sviluppo: fornisce i file statici e consente il salvataggio del catalogo e
della configurazione dall'amministrazione.

Dalla cartella `src/` (dove si trova `index.html`):

```bash
python server.py            # porta predefinita 8000
python server.py 8001       # porta alternativa
python server.py watch 8000 # supervisore: riavvia in automatico dopo un crash
```

Su Windows puoi anche fare doppio clic su **`lancio.bat`** (equivale a
`python server.py watch 8000`).

All'avvio vedrai:

```
DataPlana locale: http://localhost:8000 (home: .../src)
Admin: http://localhost:8000/admin.html (user/pass da deploy/.htpasswd)
Premi Ctrl+C per fermare.
```

## Aprire il portale

- **Portale pubblico:** <http://localhost:8000>
- **Amministrazione:** <http://localhost:8000/admin.html> — il browser chiede utente e
  password (**default `admin` / `admin`, da cambiare subito** — vedi
  [Credenziali dell'area admin](credenziali.md))
- **Privacy Policy:** <http://localhost:8000/privacy-policy.html>
- **Credits:** <http://localhost:8000/credits.html>

> **Importante:** va sempre servito tramite il web server. Aprendo `index.html` con un
> doppio clic (`file://`) i `fetch()` non funzionano e la pagina non si carica.

## Modalità `watch` (supervisore)

`python server.py watch [porta]` avvia il server come processo figlio e lo riavvia
automaticamente in caso di terminazione imprevista, registrando motivo e codice di uscita in
`server.log` e `server_error.log`. Utile per lo sviluppo continuo.

- Il supervisore si ferma con `Ctrl+C`.
- Se la porta è già occupata il server **non** riparte in automatico: chiudi l'altro
  processo oppure usa un'altra porta (`python server.py watch 8001`).

## Log

Il server scrive due file di log nella cartella `src/`:

| File | Contenuto |
|------|-----------|
| `server.log` | avvio, access log, errori e shutdown |
| `server_error.log` | solo errori e traceback, con timestamp |

## Gestione dei problemi comuni

| Problema | Soluzione |
|----------|-----------|
| `bind fallito sulla porta 8000` / "porta già in uso" | chiudi il processo che occupa la porta o usa `python server.py 8001` |
| L'admin segnala "PyYAML non installato" | esegui `python -m pip install pyyaml` nella cartella `src/` (serve solo per l'editor di configurazione) |
| Le pagine si aprono ma sono bianche | stai aprendo `index.html` da `file://`: avvia il server e usa `http://localhost:8000` |
| La home si vede ma i filtri non rispondono | verifica che `config/conf.yaml` e `config/datasets.json` siano presenti e validi |

## Prossimi passi

- [Configurazione](configurazione.md) — personalizzare il portale
- [Catalogo e amministrazione](amministrazione.md) — gestire i dataset
- [Deploy](deploy.md) — pubblicare su un dominio