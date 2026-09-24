# DataPlana

Benvenuto nella documentazione di **DataPlana**, il portale open data statico.

DataPlana è un portale open data **100% statico** (HTML + CSS + JavaScript): pubblica un
catalogo di dataset che i visitatori possono cercare, esplorare e scaricare, con una pagina
di amministrazione per gestire il catalogo senza toccare il codice. Nessun backend sul
deploy: i file sono lo storage. Il piccolo server Python incluso serve **solo per lo
sviluppo locale**.

## Caratteristiche

- **Catalogo** di dataset, ognuno con una scheda dedicata (titolo, ente, descrizione, formati).
- **Ricerca** nel catalogo e **filtri** per categoria e per formato.
- **Statistiche** (numero di dataset ed enti pubblicatori).
- **Schede dataset** con **download** dei file — un file per formato.
- **Anteprima nel browser** per JSON, GeoJSON, CSV e XML; XLSX è solo download.
- **Più formati per dataset**: ogni voce del catalogo può esporre più file; un selettore
  permette di scegliere quale esplorare, con link di download per ciascun formato.
- **Layout responsive**, nessuna libreria esterna (Font Awesome e font Titillium Web serviti
  localmente, zero richieste a CDN).
- **Amministrazione** via `admin.html`: CRUD del catalogo ed editor di configurazione.

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

## Avvio rapido

```bash
git clone https://github.com/pietrobiase/dataplana.git
cd dataplana/src
python server.py           # poi apri http://localhost:8000
```

Per l'amministrazione locale: `http://localhost:8000/admin.html` (credeniali da
`deploy/.htpasswd`). Tutti i dettagli in [Avvio rapido](avvio-rapido.md).

## Come funziona

Il portale è intenzionalmente senza database: i file sono lo storage.

- Il **catalogo** (`config/datasets.json`) e la **configurazione** (`config/conf.yaml`)
  sono file letti dal browser via `fetch()`.
- I **dati** vivono in `dataset/` come file veri (JSON, CSV, GeoJSON, XML, XLSX…).
- Il **deploy** è quindi un semplice caricamento di file statici su un web server: non serve
  un backend. Vedi [Deploy](deploy.md).
- Il server Python (`server.py`) esiste solo per lo sviluppo locale, perché consente anche
  il salvataggio del catalogo e della configurazione dall'amministrazione
  (`python -m http.server` non lo consentirebbe).

## Percorso di documentazione

- [Installazione](installazione.md) — requisiti e download dal repository GitHub
- [Avvio rapido](avvio-rapido.md) — far girare il portale in locale
- [Configurazione](configurazione.md) — riferimento completo di `conf.yaml` e dell'editor admin
- [Catalogo e amministrazione](amministrazione.md) — gestione dei dataset e schema dei metadati
- [Privacy policy](privacy.md) — come impostare la pagina Privacy Policy
- [Struttura del progetto](struttura.md) — organizzazione dei file e dei componenti
- [Deploy](deploy.md) — pubblicazione su dominio
- [Contribuire](contribuire.md) — come partecipare allo sviluppo
- [Licenza](licenza.md) — AGPL-3.0 e licenze delle risorse incluse

## Licenza

Il software del portale è distribuito sotto licenza **GNU Affero General Public License
v3.0 (AGPL-3.0)** — [gnu.org/licenses/agpl-3.0.html](https://www.gnu.org/licenses/agpl-3.0.html).
Vedi [Licenza](licenza.md) per i dettagli e le licenze delle risorse di terze parti.