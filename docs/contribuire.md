# Contribuire

Benvenuto! DataPlana è un progetto open source (AGPL-3.0) e ogni contributo è gradito,
dalla segnalazione di un bug alla proposta di una funzionalità, dalla documentazione al
codice. Questa pagina contiene le linee guida essenziali per contribuire in modo efficace.

## Prima di iniziare

- Consulta la [documentazione](index.md) per capire com'è fatto il progetto.
- Dai un'occhiata a [Struttura del progetto](struttura.md) per orientarti nei file.
- Assicurati che il portale giri in locale ([Avvio rapido](avvio-rapido.md)).

## Segnalare un bug

- Apri una **Issue** su GitHub indicando: sistema operativo e browser, versione di Python
  (se pertinente), **passi per riprodurre**, risultato atteso vs. ottenuto, ed eventuali
  righe di `server.log`/`server_error.log`.
- Controlla prima se l'Issue esiste già: se sì, aggiungi un commento invece di aprirne
  un'altra.
- Per un bug relativo alla documentazione, indica la pagina interessata.

## Proporre una funzionalità

- Apri un'Issue **"Feature request"** descrivendo il problema che vuoi risolvere e l'uso
  che ne farai, prima di scrivere codice: il mantenitore può suggerirti una soluzione più
  aderente all'architettura del progetto.
- Le indicazioni dell'Issue vengono poi tradotte in una pull request (vedi sotto).

## Convenzioni di codice

DataPlana adotta scelte deliberate, da rispettare per mantenere l'identità del progetto:

- **JavaScript ES5** puro: nessun framework, nessuna libreria, nessun bundler, nessuna
  dipendenza da CDN. Compatibilità massima prima di tutto.
- **Nessuna libreria esterna a runtime**: font (Titillium Web) e icone (Font Awesome) sono
  venduti (già scaricati) in `fonts/` e `vendor/`.
- **CSS**: un unico file `css/styles.css` con design tokens (variabili `--g05…--g95`,
  `--ink`…); nessun framework CSS o preprocessore.
- **Commenti in italiano** e codice leggibile; le stringhe mostrate all'utente sono in
  italiano.
- **Scritture atomiche**: sul server, i file `config/` vengono scritti con file temporaneo +
  `os.replace()` per non lasciare mai il file a metà.
- **Niente log pieni di dati sensibili**; i dati dei log restano in `server.log` e
  `server_error.log`.

## Workflow con Git

1. **Fork** il repository su GitHub e clona il tuo fork:
   ```bash
   git clone https://github.com/TUO-UTENTE/dataplana.git
   cd dataplana
   ```
2. Crea un **branch** per la tua modifica:
   ```bash
   git checkout -b fix/nome-significativo
   ```
3. Fai le modifiche e **verifica** il funzionamento in locale
   ([Avvio rapido](avvio-rapido.md)) e, se toccano la documentazione, il build di MkDocs
   ([Documentazione](#documentazione)).
4. Fai commit con messaggi chiari in italiano o inglese.
5. **Push** e apri una **pull request** verso il branch `main` del repository originale,
   descrivendo cosa cambia e perché.

## Documentazione

La documentazione è un sito **MkDocs** (vedi `mkdocs.yml` e `docs/`), pubblicato su
ReadTheDocs. Per lavorarci:

```bash
cd dataplana
python -m pip install -r docs/requirements.txt
mkdocs serve        # anteprima locale su http://127.0.0.1:8000
mkdocs build        # verifica finale
```

Applica la stessa qualità dei contenuti: struttura chiara, tabelle per i riferimenti,
link interni coerenti.

## Linee guida per i dataset di esempio

I dataset in `dataset/` sono dimostrativi: piccoli, corretti e descritti nei metadati.
Quando aggiungi un dataset di esempio:

- usa formati già gestibili in `conf.yaml` (JSON, GeoJSON, CSV, XML, XLSX);
- mantieni l'ID `kebab-case` (es. `comuni-italiani`) e un `updated` in formato `YYYY-MM-DD`;
- se citi enti reali (ISTAT, ARPA…) etichettali come rielaborazioni dimostrative.

## Licenza dei contributi

Il progetto è distribuito sotto **AGPL-3.0**; contribuendo, accetti che le tue modifiche
vengano distribuite con la stessa licenza (vedi [Licenza](licenza.md)).

## Codice di comportamento

Sii rispettoso e costruttivo nelle discussioni. Il progetto è piccolo: ogni contributo
conteggia, purché allineato alle scelte architetturali documentate qui.