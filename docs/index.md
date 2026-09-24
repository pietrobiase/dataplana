# DataPlana

Benvenuto nella documentazione di **DataPlana**, il portale open data statico.

DataPlana è un prototipo di portale open data 100% statico (HTML + CSS + JavaScript) con
una pagina di amministrazione per la gestione del catalogo. Nessun backend sul deploy:
i file sono lo storage. Il server Python presente nel progetto serve **solo per lo
sviluppo locale**.

## Caratteristiche principali

- ricerca nel catalogo
- filtro per categoria e per formato
- statistiche (dataset, formati)
- schede dataset con download dei file (un file per formato)
- anteprima nel browser: **JSON, GeoJSON, CSV, XML**
  (per **XLSX** compare "Anteprima non disponibile…" e si invita a scaricare il file)
- ogni dataset può avere **uno o più formati**: quando i file sono più di uno un
  selettore permette di scegliere quale esplorare, con link di download per ciascun formato
- layout responsive, nessuna libreria esterna (Font Awesome e font Titillium Web serviti localmente)

## Licenza

Il software del portale è distribuito sotto licenza **GNU Affero General Public License
v3.0 (AGPL-3.0)** — [gnu.org/licenses/agpl-3.0.html](https://www.gnu.org/licenses/agpl-3.0.html).

## Esplora la documentazione

- [Struttura del portale](struttura.md) — organizzazione dei file e dei componenti
- [Amministrazione](amministrazione.md) — gestione del catalogo e credenziali
- [Deploy](deploy.md) — messa in produzione su hosting web