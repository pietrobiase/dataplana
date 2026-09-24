# Struttura del progetto

Questa pagina descrive l'organizzazione del repository e dei componenti del portale.

## Struttura del repository

```
dataplana/
├── src/                        # il portale (tutto ciò che va pubblicato)
│   ├── index.html              portale pubblico (home, schede categorie)
│   ├── catalogo.html           catalogo: ricerca, filtri, anteprime
│   ├── admin.html              pagina di amministrazione (protetta in deploy)
│   ├── privacy-policy.html     pagina Privacy Policy
│   ├── credits.html            pagina Credits (risorse e attribuzioni)
│   ├── css/styles.css          stili condivisi
│   ├── fonts/titillium-web/    font Titillium Web locali (woff2/woff — licenza SIL OFL 1.1)
│   ├── vendor/font-awesome/    Font Awesome Free 6.5.2 scaricato in locale (CSS + webfonts)
│   ├── components/
│   │   ├── header.html         header unico iniettato da js/header.js
│   │   └── footer.html         footer unico iniettato da js/footer.js
│   ├── js/
│   │   ├── parsers.js          utilità condivise (esc, CSV/JSON/GeoJSON/XML, mini-YAML, Markdown)
│   │   ├── header.js           inietta components/header.html in <div id="site-header">
│   │   ├── footer.js           inietta components/footer.html in <footer id="site-footer">
│   │   ├── app.js              logica della pagina catalogo.html (ricerca, filtri, anteprime)
│   │   ├── index.js            home: schede delle categorie che aprono catalogo.html?categoria=…
│   │   ├── admin.js            logica di amministrazione
│   │   ├── privacy.js          pagina Privacy Policy
│   │   └── credits.js          pagina Credits (config dinamica + crediti utente)
│   ├── config/
│   │   ├── conf.yaml           configurazione portale (titolo, SEO, footer, links, crediti
│   │   │                       utente, pageSize, colore base, categorie + icone,
│   │   │                       Font Awesome, formati gestibili) — vedi Configurazione
│   │   ├── datasets.json       catalogo degli open data (metadati)
│   │   └── privacy-policy.md   contenuto della pagina Privacy Policy — vedi Privacy policy
│   ├── dataset/                i file reali degli open data (JSON, CSV, GeoJSON, XML, XLSX…)
│   ├── deploy/
│   │   ├── .htaccess           regole di deploy (https://www.sovranita-digitale.it/dataset)
│   │   └── .htpasswd           credenziali admin (SHA-512 crypt)
│   ├── local/
│   │   ├── .htaccess           regole per il web server locale (Apache/XAMPP/MAMP)
│   │   └── .htpasswd           credenziali admin (stessa coppia utente/password)
│   ├── server.py               SOLO sviluppo locale: statici + auth + salvataggio catalogo
│   ├── lancio.bat              avvio rapido in locale (Windows)
│   └── credenziali.txt         memo locale con utente/password (da NON pubblicare)
├── docs/                       questa documentazione (MkDocs / ReadTheDocs)
├── mkdocs.yml                  configurazione della documentazione
├── README.md
└── LICENSE                     AGPL-3.0
```

Il resto del repository esiste per la pubblicazione del progetto open source; **tutto ciò
che serve al portale sta in `src/`**.

## Componenti condivisi

`components/header.html` e `components/footer.html` vengono iniettati rispettivamente da
`js/header.js` e `js/footer.js`. Il footer include la dichiarazione di licenza del portale.

- `header.js` rende la barra di navigazione condivisa e offre l'hook `fillHeader()` (usato
  da `admin.js` per il titolo della pagina).
- `footer.js` si limita a iniettare il footer.
- `parsers.js` espone l'oggetto globale `OpenData` con parser CSV/JSON/GeoJSON/XML, il
  mini-parser YAML per `conf.yaml`, il renderer Markdown e gli helper per icone/categorie.

## Dati e configurazione

- **`config/conf.yaml`** — configurazione del portale. Consulta la pagina
  [Configurazione](configurazione.md) per il riferimento completo delle chiavi.
- **`config/datasets.json`** — catalogo dei dataset. Consulta la pagina
  [Catalogo e amministrazione](amministrazione.md) per lo schema dei metadati.
- **`config/privacy-policy.md`** — contenuto della pagina Privacy Policy (vedi
  [Privacy policy](privacy.md)).

## Formati supportati e anteprima

| Formato | Anteprima | Download |
|---------|-----------|----------|
| JSON    | sì        | sì       |
| GeoJSON | sì        | sì       |
| CSV     | sì        | sì       |
| XML     | sì        | sì       |
| XLSX    | no        | sì       |

L'elenco dei formati gestibili è configurabile in `conf.yaml` (`formats`): aggiungere o
rimuovere un formato lì controlla le opzioni disponibili nell'admin e i badge nel portale.
L'anteprima XLSX richiederebbe una libreria lato client (es. SheetJS/xlsx) non inclusa
volutamente: il file si scarica e si apre con un applicativo dedicato.