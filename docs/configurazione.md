# Configurazione

Quasi tutto l'aspetto e il comportamento del portale si controllano da un unico file:
**`config/conf.yaml`**. Il catalogo dei dataset, invece, vive in **`config/datasets.json`**
e viene trattato nella pagina [Catalogo e amministrazione](amministrazione.md).

Ci sono **due modi equivalenti** per modificare la configurazione:

1. **Modifica diretta del file `config/conf.yaml`** (consigliata, funziona sempre, anche su
   hosting statico dove non c'è il server di sviluppo);
2. **Editor di configurazione in `admin.html`** (comodo, ma per salvare richiede il server
   di sviluppo locale con PyYAML, o WebDAV in produzione).

## Come viene letto il file

Il browser scarica `config/conf.yaml` all'apertura di ogni pagina e lo interpreta con il
mini-parser YAML di `js/parsers.js` (`parseYAML`), quindi **non serve riavviare nulla**:
basta ricaricare la pagina. Le voci possono essere commentate o rimosse: per molte voci
esiste un valore predefinito nel CSS/JS. La mancata corrispondenza di una categoria o di un
formato si riflette soltanto nell'elenco di opzioni dell'admin.

## 1. Modifica diretta di `config/conf.yaml`

Apri il file con un editor di testo. Nomi e tipi delle chiavi sono fissi; l'ordine è libero.
Le chiavi riconosciute sono esattamente: `title`, `subtitle`, `basedir`, `baseColor`,
`pageSize`, `seo`, `footer`, `links`, `categories`, `fontAwesome`, `formats`, `credits`.

Un esempio commentato e uguale a quello fornito è già presente nel file. Di seguito la
descrizione puntuale di ogni parametro.

### `title` — *stringa*

Nome del portale, mostrato nel brand in alto e usato come titolo della sezione del footer.

```yaml
title: DataPlana
```

### `subtitle` — *stringa*

Sottotitolo del portale, mostrato sotto il brand nell'header.

```yaml
subtitle: Open data, made light.
```

### `basedir` — *stringa (percorso cartella, es. `/nomecartella`)*

Cartella del portale sul server. Se il sito è pubblicato in una **sottocartella**
(es. `https://esempio.it/nomecartella/`), indicare `/nomecartella`: il pulsante **Home**, il
logo e il menu puntano alla home corretta invece che alla root del dominio. Vuoto oppure
commentato = il portale sta nella root (valore predefinito).

```yaml
basedir: ''
```

```yaml
basedir: /nomecartella
```

### `baseColor` — *hex `#rrggbb` o `rrggbb`*

Colore base della palette. Dato questo colore, all'avvio il portale genera le **11
gradazioni** (`05…95`) usate per l'interfaccia con una regola universale: stessa tinta (H) e
stessa saturazione (S) del colore base, mentre la chiarezza (L) viene interpolata verso il
bianco (`05–40`) e verso il nero (`60–95`), passando per il colore base al livello 50.

```yaml
baseColor: '#187abc'
```

Se assente o non valido, restano i valori predefiniti del CSS (`#187abc`).

### `pageSize` — *intero ≥ 1*

Anteprima dati (esplora dataset): quante righe mostrare a ogni click sul pulsante
**"Mostra altri valori"**. Se il dataset ha meno righe di questo valore, il pulsante non
compare (si attiva solo quando i record superano la pagina).

```yaml
pageSize: 10
```

### `seo` — *oggetto con `title`, `description`, `keywords`*

Contenuto dei meta tag SEO. Se manca `description`, viene usato il sottotitolo del sito.

```yaml
seo:
  title: DataPlana — Catalogo dei dati aperti
  description: Open data, made light. Catalogo dei dati aperti del tuo ente, senza librerie esterne né backend.
  keywords: open data, dati aperti, catalogo, opendata
```

| Chiave | Tipo | Descrizione |
|--------|------|-------------|
| `title` | stringa | `<title>` e `og:title` |
| `description` | stringa | `meta description`; se vuota si usa il sottotitolo |
| `keywords` | stringa | `meta keywords` |

### `footer` — *oggetto con `title`, `description`*

Prima sezione a sinistra del footer (la colonna di sinistra del riquadro di chiusura).

```yaml
footer:
  title: DataPlana
  description: Open data, made light. Catalogo dei dati aperti del tuo ente, senza librerie esterne né backend.
```

### `links` — *lista di oggetti `{title, url}`*

Sezione **"Links"** del footer (a destra): una voce per riga. Ogni voce è un link mostrato
in fondo alla pagina.

```yaml
links:
  - title: Privacy Policy
    url: privacy-policy.html
  - title: Credits
    url: credits.html
```

| Chiave | Tipo | Descrizione |
|--------|------|-------------|
| `title` | stringa | testo visibile del link |
| `url` | stringa | destinazione (relativa o assoluta) |

### `categories` — *lista di oggetti `{label, icon}`*

**Categorie gestibili**: popolano il menu a tendina di `admin.html` e le icone nel portale.
Ogni voce corrisponde a una categoria selezionabile nel campo `category` di una scheda
dataset; `label` è il valore effettivamente salvato nel catalogo.

```yaml
categories:
  - label: Territorio
    icon: fa-solid fa-map-location-dot
  - label: Ambiente
    icon: fa-solid fa-leaf
  - label: Cultura
    icon: fa-solid fa-book
```

| Chiave | Tipo | Descrizione |
|--------|------|-------------|
| `label` | stringa | nome della categoria (salvato nel catalogo) |
| `icon` | stringa | classe Font Awesome mostrata accanto al nome |

### `fontAwesome` — *stringa (percorso CSS)*

Risorsa Font Awesome utilizzata per le icone delle categorie e dei formati. Il file è
scaricato in locale nella cartella `vendor/font-awesome`. **Commenta la riga per disattivare
le icone.**

```yaml
fontAwesome: vendor/font-awesome/css/all.min.css
```

### `formats` — *lista di oggetti `{format, preview, icon}`*

**Formati gestibili**: popolano il menu a tendina di `admin.html` e la sezione "Formati
supportati" del footer. `format` è il valore salvato nel campo `format` della scheda
dataset; `preview` indica se il portale offre l'anteprima nel browser; `icon` è la classe
Font Awesome mostrata accanto al formato.

```yaml
formats:
  - format: JSON
    preview: true
    icon: fa-solid fa-file-code
  - format: GeoJSON
    preview: true
    icon: fa-solid fa-draw-polygon
  - format: CSV
    preview: true
    icon: fa-solid fa-file-csv
  - format: XML
    preview: true
    icon: fa-solid fa-code
  - format: XLSX
    preview: false
    icon: fa-solid fa-file-excel
```

| Chiave | Tipo | Descrizione |
|--------|------|-------------|
| `format` | stringa | nome del formato come salvato nel catalogo |
| `preview` | booleano | `true` = anteprima nel browser; `false` = solo download |
| `icon` | stringa | classe Font Awesome mostrata accanto al formato |

### `credits` — *lista di oggetti `{testo_icona, titolo, licenza, descrizione, link}`*

**Crediti definiti dall'utente**: ogni voce disegna una scheda in fondo alla pagina Credits,
sotto il paragrafo "Credits definiti dall'utente". La sezione compare solo se c'è almeno una
voce completa (con `titolo`); le voci senza `titolo` vengono ignorate automaticamente.

```yaml
credits:
  - testo_icona: ISTAT
    titolo: Rielaborazione ISTAT
    licenza: CC BY 4.0
    descrizione: Rielaborazione dimostrativa su dati pubblici ISTAT.
    link: https://www.istat.it
```

| Chiave | Tipo | Descrizione |
|--------|------|-------------|
| `testo_icona` | stringa | abbreviazione mostrata sull'icona della scheda (es. "PIA") |
| `titolo` | stringa | titolo del credito (obbligatorio; senza titolo la voce è ignorata) |
| `licenza` | stringa | badge della licenza (es. "CC BY 4.0") |
| `descrizione` | stringa | testo descrittivo della scheda |
| `link` | stringa | URL facoltativo mostrato in fondo alla scheda |

## 2. Campi corrispondenti dell'editor admin

Il pannello **"Configurazione"** di `admin.html` espone gli stessi parametri in un modulo
grafico. La mappa tra i campi del modulo e le chiavi di `conf.yaml`:

| Pannello admin | Chiave `conf.yaml` |
|----------------|--------------------|
| Campo "Titolo" | `title` |
| Campo "Sottotitolo" | `subtitle` |
| Campo "Cartella del portale (BASEDIR)" | `basedir` |
| Campo "Colore base" (+ selettore colore) | `baseColor` |
| Campo "Righe per anteprima" | `pageSize` |
| Riquadro SEO → Titolo / Descrizione / Keywords | `seo` → `title`, `description`, `keywords` |
| Riquadro Footer → Titolo / Descrizione | `footer` → `title`, `description` |
| Riquadro "Links" (righe titolo/url) | `links` (righe `{title, url}`) |
| Riquadro "Categorie" (righe label/icona) | `categories` (righe `{label, icon}`) |
| Riquadro "Formati" (righe formato/anteprima/icona) | `formats` (righe `{format, preview, icon}`) |
| Riquadro "Credits" (righe icona/titolo/licenza/descrizione/link) | `credits` (righe `{testo_icona, titolo, licenza, descrizione, link}`) |

Il campo "Font Awesome (CSS)" dell'editor corrisponde alla chiave `fontAwesome`.

## Come salvare la configurazione

### In locale (sviluppo)

1. Apri <http://localhost:8000/admin.html>.
2. Vai alla sezione **"Configurazione"** e modifica i campi.
3. Premi **"Aggiorna"**: il browser invia un `PUT` autenticato a `config/conf.yaml`. Il
   `server.py` convalida i valori (chiavi riconosciute, `pageSize` ≥ 1, liste non vuote),
   rigenera il file YAML completo di commenti e lo salva in modo atomico.
   Richiede **PyYAML** installato, altrimenti ricevi il messaggio
   "PyYAML non installato: eseguire `pip install pyyaml`".

### In produzione (hosting statico)

Il salvataggio online via `PUT` richiede il server di sviluppo oppure **WebDAV** abilitato
sull'hosting. In assenza di WebDAV, modifica `config/conf.yaml` direttamente sul tuo
computer e caricalo via FTP nella cartella `config/` (vedi [Deploy](deploy.md)). Il formato
del file è lo stesso nei due casi.

## Note

- **Il browser legge il file a ogni pagina**: dopo aver salvato/modificato, ricarica il
  portale o l'admin con `Ctrl+F5` per aggirare la cache.
- **Non rinominare** le chiavi: il resto della applicazione assume i nomi qui documentati.
- **`datasets.json` è separato**: le voci del catalogo (dataset) si gestiscono come descritto
  in [Catalogo e amministrazione](amministrazione.md).