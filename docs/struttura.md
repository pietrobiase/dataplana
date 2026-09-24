# Struttura del portale

Il portale vive interamente in una cartella di file statici. Struttura di riferimento:

```
dataplana/
├── index.html            portale pubblico
├── admin.html            pagina di amministrazione (protetta in deploy)
├── privacy-policy.html   pagina Privacy Policy
├── credits.html          pagina Credits (risorse e attribuzioni)
├── css/styles.css        stili condivisi
├── fonts/titillium-web/  font Titillium Web locali (woff2/woff — licenza SIL OFL 1.1)
├── vendor/font-awesome/  Font Awesome Free 6.5.2 scaricato in locale (CSS + webfonts)
├── components/
│   ├── header.html       header unico iniettato da js/header.js
│   └── footer.html       footer unico iniettato da js/footer.js
├── js/
│   ├── parsers.js        utilità condivise (esc, CSV/JSON/GeoJSON/XML, XLSX)
│   ├── header.js         inietta components/header.html in <div id="site-header">
│   ├── footer.js         inietta components/footer.html in <footer id="site-footer">
│   ├── app.js            logica della pagina catalogo.html (ricerca, filtri, anteprime)
│   ├── index.js          home: schede delle categorie che aprono catalogo.html?categoria=…
│   ├── admin.js          logica di amministrazione
│   ├── privacy.js        pagina Privacy Policy
│   └── credits.js        pagina Credits (config dinamica + crediti utente)
├── config/
│   ├── conf.yaml         configurazione portale (titolo, SEO, footer, links, crediti utente,
│   │                     pageSize, colore base, categorie + icone, Font Awesome, formati gestibili)
│   ├── datasets.json     catalogo degli open data (metadati)
│   └── privacy-policy.md contenuto della pagina Privacy Policy
├── dataset/              i file reali degli open data (JSON, CSV, GeoJSON, XML, XLSX…)
├── deploy/
│   ├── .htaccess         regole di deploy (https://www.sovranita-digitale.it/dataset)
│   └── .htpasswd         credenziali admin (SHA-512 crypt)
├── local/
│   ├── .htaccess         regole per il web server locale (Apache/XAMPP/MAMP)
│   └── .htpasswd         credenziali admin (stessa coppia utente/password)
├── server.py             SOLO sviluppo locale: statici + auth + salvataggio catalogo
├── lancio.bat            avvio rapido in locale
└── credenziali.txt       memo locale con utente/password (da NON pubblicare)
```

## Componenti condivisi

`components/header.html` e `components/footer.html` vengono iniettati rispettivamente da
`js/header.js` e `js/footer.js`. Il footer include la dichiarazione di licenza del portale.

## Configurazione (`config/conf.yaml`)

Il file `conf.yaml` controlla: titolo e meta SEO, voci di footer e links, crediti utente,
`pageSize`, colore base, categorie con relative icone Font Awesome, formati gestibili.
Il catalogo dei dataset è in `config/datasets.json`.

## Formati e anteprima

| Formato | Anteprima | Download |
|---------|-----------|----------|
| JSON    | sì        | sì       |
| GeoJSON | sì        | sì       |
| CSV     | sì        | sì       |
| XML     | sì        | sì       |
| XLSX    | no        | sì       |

L'anteprima XLSX richiederebbe una libreria lato client (es. SheetJS/xlsx) non inclusa
volutamente: il file si scarica e si apre con un applicativo dedicato.