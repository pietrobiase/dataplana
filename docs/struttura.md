# Struttura del progetto

Questa pagina descrive l'organizzazione del repository e dei componenti del portale.

## Struttura del repository

```text
dataplana/
├── .gitattributes      Configurazione Git (line endings)
├── .gitignore          Ignora *.bak e src/ (codice app non versionato in questo repo)
├── LICENSE             Licenza AGPL-3.0
├── README.md           Presentazione del progetto
├── mkdocs.yml          Configurazione MkDocs
├── readthedocs.yaml    Configurazione build ReadTheDocs
├── docs/               Documentazione MkDocs/ReadTheDocs
│   ├── amministrazione.md
│   ├── avvio-rapido.md
│   ├── configurazione.md
│   ├── contribuire.md
│   ├── credenziali.md
│   ├── deploy.md
│   ├── index.md
│   ├── installazione.md
│   ├── licenza.md
│   ├── privacy.md
│   ├── requirements.txt
│   └── struttura.md
└── src/                Portale statico (tutto ciò che va pubblicato)
```

## Struttura della cartella `src/`

```text
src/
├── admin.html              Amministrazione (protetta in deploy)
├── catalogo.html           Catalogo pubblico
├── credits.html            Credits
├── credits.html.bak        Backup precedente (non pubblicare)
├── index.html              Home
├── lancio.bat              Avvio rapido Windows
├── logo.png                Logo
├── privacy-policy.html     Privacy Policy
├── server.py               Solo sviluppo locale (statici + auth + PUT)
├── server.log              Log sviluppo
├── server_error.log        Log errori sviluppo
├── __pycache__/            Cache Python
├── components/
│   ├── footer.html         Footer iniettato
│   └── header.html         Header iniettato
├── config/
│   ├── conf.yaml           Configurazione portale
│   ├── datasets.json       Catalogo
│   └── privacy-policy.md   Contenuto Privacy Policy
├── css/
│   └── styles.css          Stili
├── dataset/                File dati (JSON, CSV, GeoJSON, XML, XLSX...)
├── deploy/
│   ├── .htaccess           Apache produzione
│   └── .htpasswd           Credenziali admin (SHA-512 crypt)
├── fonts/
│   └── titillium-web/      Font Titillium Web (OFL 1.1)
├── js/
│   ├── admin.js            Logica admin
│   ├── app.js              Logica catalogo
│   ├── credits.js          Logica credits
│   ├── footer.js           Iniezione footer
│   ├── header.js           Iniezione header
│   ├── index.js            Logica home
│   ├── parsers.js          Parser/renderer YAML/Markdown/CSV/JSON/XML
│   └── privacy.js          Logica privacy
├── local/
│   ├── .htaccess           Apache test locale
│   └── .htpasswd           Credenziali test Apache
└── vendor/
    └── font-awesome/       Font Awesome Free 6.5.2 locale
```

## Componenti condivisi

`components/header.html` e `components/footer.html` vengono iniettati rispettivamente da
`js/header.js` e `js/footer.js`. Il footer include la dichiarazione di licenza del portale.

## Dati e configurazione

- **`config/conf.yaml`** — configurazione del portale ([Configurazione](configurazione.md))
- **`config/datasets.json`** — catalogo dei dataset ([Catalogo e amministrazione](amministrazione.md))
- **`config/privacy-policy.md`** — contenuto della pagina Privacy Policy ([Privacy policy](privacy.md))

## Formati supportati e anteprima

| Formato | Anteprima | Download |
|---------|-----------|----------|
| JSON    | sì        | sì       |
| GeoJSON | sì        | sì       |
| CSV     | sì        | sì       |
| XML     | sì        | sì       |
| XLSX    | no        | sì       |