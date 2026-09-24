# Sviluppo locale e Deploy

## Avvio in locale

1. Doppio clic su `lancio.bat` (oppure dalla cartella: `python server.py 8000`).
2. Apri `http://localhost:8000` per il portale pubblico.
3. Apri `http://localhost:8000/admin.html` per l'amministrazione
   (il browser chiede le credenziali: vedi [Amministrazione](amministrazione.md)).

> Servire i file solo tramite web server: `fetch()` non funziona aprendo `index.html` da `file://`.

## Deploy

Il portale verrà servito su **https://www.sovranita-digitale.it/dataset/**: carica tutti i
file dentro la directory dell'hosting mappata su quel percorso.

```
index.html
admin.html
css/
js/
config/
dataset/
deploy/.htaccess   → come ".htaccess" nella root del sito
deploy/.htpasswd   → nella posizione indicata dentro .htaccess
```

`.htaccess` fornisce: `Options -Indexes`, header di sicurezza, blocco dell'accesso via web a
`.htaccess`/`.htpasswd`/`credenziali.txt` e Basic Auth solo su `admin.html`. Adatta
`AuthUserFile` al percorso assoluto reale del tuo hosting (es. cPanel:
`/home/UTENTE/public_html/sovranita-digitale.it/dataset/.htpasswd`).

### Coppie di .htaccess/.htpasswd

Sono presenti due coppie identiche per credenziali (utente `hq`, password in `credenziali.txt`):

| Cartella | Uso | AuthUserFile da impostare |
|----------|-----|---------------------------|
| `deploy/` | hosting di produzione (www.sovranita-digitale.it/dataset) | percorso assoluto sul server |
| `local/`  | Apache in locale (XAMPP/MAMP) per test | percorso assoluto locale (es. `C:/xampp/htdocs/...`) |

Il web server locale predefinito (`server.py`) usa `deploy/.htpasswd` senza bisogno di
`.htaccess`; la coppia in `local/` serve solo se si preferisce testare con Apache reale.

**Non caricare mai**: `server.py`, `lancio.bat`, `credenziali.txt` e la cartella `local/`.

## Evoluzione prevista

Una REST API pubblica (read) è prevista come evoluzione futura, non inclusa in questa versione.