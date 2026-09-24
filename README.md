# Open Data Hub

Prototipo di portale open data 100% statico (HTML + CSS + JavaScript) con pagina di
amministrazione per la gestione del catalogo. Nessun backend sul deploy: i file sono
lo storage. Il server Python presente in questo progetto serve **solo per lo sviluppo locale**.

## Avvio in locale

1. Doppio clic su `lancio.bat` (oppure dalla cartella: `python server.py 8000`).
2. Apri `http://localhost:8000` per il portale pubblico.
3. Apri `http://localhost:8000/admin.html` per l'amministrazione
   (il browser chiede le credenziali: vedi sotto).

Servire i file solo tramite web server: `fetch()` non funziona aprendo `index.html` da `file://`.

## Credenziali

- Utente: `hq`
- Password: vedi `credenziali.txt` (memo locale — **non pubblicare** questo file).

L'autenticazione viene eseguita contro `deploy/.htpasswd` (formato `$6$` = SHA-512 crypt),
un formato compatibile con `htpasswd` di Apache per il deploy.

## Struttura

```
open-data-repository/
├── index.html          portale pubblico
├── admin.html          pagina di amministrazione (protetta in deploy)
├── privacy-policy.html pagina Privacy Policy
├── credits.html        pagina Credits (risorse e attribuzioni)
├── css/styles.css      stili condivisi
├── fonts/titillium-web/   font Titillium Web locali (woff2/woff — licenza SIL OFL 1.1)
├── vendor/font-awesome/   Font Awesome Free 6.5.2 scaricato in locale (CSS + webfonts)
├── components/header.html  header unico iniettato da js/header.js
├── components/footer.html  footer unico iniettato da js/footer.js
├── js/
│   ├── parsers.js      utilita' condivise (esc, CSV/JSON/GeoJSON/XML, XLSX)
│   ├── header.js       inietta components/header.html in <div id="site-header">
│   ├── footer.js       inietta components/footer.html in <footer id="site-footer">
│   ├── app.js          logica della pagina catalogo.html (ricerca, filtri, anteprime)
│   ├── index.js        home: schede delle categorie che aprono catalogo.html?categoria=…
│   ├── admin.js        logica di amministrazione
│   ├── privacy.js      pagina Privacy Policy
│   └── credits.js      pagina Credits (config dinamica + crediti utente)
├── config/
│   ├── conf.yaml       configurazione portale (titolo, SEO, footer, links, crediti utente, pageSize, colore base, categorie + icone, Font Awesome, formati gestibili)
│   ├── datasets.json   catalogo degli open data (metadati)
│   └── privacy-policy.md   contenuto della pagina Privacy Policy
├── dataset/            i file reali degli open data (JSON, CSV, GeoJSON, XML, XLSX…)
├── deploy/
│   ├── .htaccess       regole di deploy (https://www.sovranita-digitale.it/dataset)
│   └── .htpasswd       credenziali admin (SHA-512 crypt)
├── local/
│   ├── .htaccess       regole per il web server locale (Apache/XAMPP/MAMP)
│   └── .htpasswd       credenziali admin (stessa coppia utente/password)
├── server.py           SOLO sviluppo locale: statici + auth + salvataggio catalogo
├── lancio.bat          avvio rapido in locale
└── credenziali.txt     memo locale con utente/password (da NON pubblicare)
```

## Funzioni del portale

- ricerca
- filtro per categoria e per formato
- statistiche (dataset, formati)
- schede dataset con download dei file (un file per formato)
- anteprima nel browser: **JSON, GeoJSON, CSV, XML**
  (per **XLSX** compare il messaggio "Anteprima non disponibile…" e si invita a scaricare il file)
- ogni dataset può avere **uno o più formati**: nel catalogo ogni voce contiene
  `files: [{ "format": "…", "file": "dataset/…" }, …]`; quando i file sono più di uno
  un selettore permette di scegliere quale esplorare, con link di download per ciascun formato
- layout responsive, nessuna libreria esterna

## Formati e anteprima

| Formato        | Anteprima | Download |
|----------------|-----------|----------|
| JSON           | sì        | sì       |
| GeoJSON        | sì        | sì       |
| CSV            | sì        | sì       |
| XML            | sì        | sì       |
| XLSX           | no        | sì       |

L'anteprima XLSX richiederebbe una libreria lato client (es. SheetJS/xlsx) non inclusa
volutamente: il file si scarica e si apre con un applicativo dedicato.

## Workflow di aggiornamento dei dati

1. Carica i **file** in `dataset/` tramite FTP sul server.
2. Apri `admin.html` e aggiorna i **metadati** (titolo, ente, formati, percorsi file…).
   Nell'editor "File del dataset" aggiungi una riga per ogni formato (es. JSON e CSV dello stesso dataset).
3. Salva: in locale la modifica viene scritta in `config/datasets.json` dal `server.py`;
   sul deploy il salvataggio online richiede **WebDAV** abilitato, altrimenti usa
   il pulsante **"Esporta catalogo"** e ricarica il file `datasets.json` via FTP in `config/`.

Eliminare una voce dal catalogo NON cancella il file in `dataset/`.

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

**Coppie di .htaccess/.htpasswd**

Sono presenti due coppie identiche per credenziali (utente `hq`, password in `credenziali.txt`):

| Cartella | Uso | AuthUserFile da impostare |
|----------|-----|---------------------------|
| `deploy/` | hosting di produzione (www.sovranita-digitale.it/dataset) | percorso assoluto sul server |
| `local/`  | Apache in locale (XAMPP/MAMP) per test | percorso assoluto locale (es. `C:/xampp/htdocs/...`) |

Il web server locale predefinito (`server.py`) usa `deploy/.htpasswd` senza bisogno di
`.htaccess`; la coppia in `local/` serve solo se si preferisce testare con Apache reale.

**Non caricare mai**: `server.py`, `lancio.bat`, `credenziali.txt` e la cartella `local/`;

## Note di sicurezza

- `server.py` è solo per sviluppo locale (`python -m http.server` non consentirebbe il salvataggio del catalogo).
- Le password in `.htpasswd` sono hash SHA-512 crypt (non in chiaro).
- Il progetto è intenzionalmente senza database: i file JSON sono lo storage.
- Una REST API pubblica (read) è prevista come evoluzione futura, non incluso in questa versione.