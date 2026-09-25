# DataPlana

**DataPlana** è un portale open data statico: catalogo, ricerca, anteprime e download di
dataset pubblici senza alcun backend.

Tutto il portale è composto da **HTML, CSS e JavaScript**. Non c'è database: i dati e i
metadati vivono in file (JSON, YAML, CSV…, serviti direttamente dal web server). Un piccolo
server Python è incluso solo per lo sviluppo locale; sul deploy basta un qualsiasi hosting
statico.

## Cosa fa

- pubblica un **catalogo di open data**, ogni dataset con la propria scheda
- **ricerca** e **filtri** per categoria e formato
- **anteprime nel browser** per JSON, GeoJSON, CSV e XML
- **download** dei file in tutti i formati disponibili per ogni dataset
- **pagina di amministrazione** per aggiornare il catalogo e la configurazione senza toccare il codice
- layout **responsive**, senza librerie esterne (font e icone serviti localmente)

## Avvio rapido

```bash
git clone https://github.com/pietrobiase/dataplana.git
cd dataplana/src
python server.py           # poi apri http://localhost:8000
```

Admin locale: <http://localhost:8000/admin.html>. Credenziali di default: utente `admin`,
password `admin` — **da cambiare subito** (vedi la pagina "Credenziali dell'area admin" nella
[documentazione](https://dataplana.readthedocs.io/) o `docs/credenziali.md`).

Per usare anche l'editor di configurazione in locale: `python -m pip install pyyaml`.

## Deploy

Carica i file della cartella `src/` su un hosting statico (vedi la pagina [Deploy]).
`sovranita-digitale.it/dataset/` è un esempio di destinazione; il portale funziona su
qualunque hosting (Apache con `.htaccess` consigliato per la protezione di `admin.html`).

## Documentazione

Tutti i dettagli — installazione, configurazione, amministrazione e deploy — sono nella
[documentazione](https://dataplana.readthedocs.io/). I capitoli principali:

- [Installazione](docs/installazione.md)
- [Avvio rapido](docs/avvio-rapido.md)
- [Configurazione](docs/configurazione.md)
- [Catalogo e amministrazione](docs/amministrazione.md)
- [Privacy policy](docs/privacy.md)
- [Struttura del progetto](docs/struttura.md)
- [Deploy](docs/deploy.md)
- [Contribuire](docs/contribuire.md)
- [Licenza](docs/licenza.md)

## Licenza

Distribuito con licenza **GNU Affero General Public License v3.0 (AGPL-3.0)** — vedi
[LICENSE](LICENSE) e [gnu.org/licenses/agpl-3.0.html](https://www.gnu.org/licenses/agpl-3.0.html).