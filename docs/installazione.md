# Installazione

Questa pagina spiega come ottenere e predisporre DataPlana partendo dal repository GitHub,
in due scenari:

1. **Solo portale statico** — nessuna installazione: basta clonare e pubblicare i file
   statici (`src/`) su un hosting.
2. **Portale + sviluppo locale** — occorre Python per usare il server incluso
   (`server.py`) che abilita il salvataggio del catalogo dall'amministrazione.

## Requisiti

| Risorsa | Necessaria per | Note |
|---------|----------------|------|
| `git`   | clonare il repository | qualsiasi versione moderna |
| Python 3.8+ | sviluppo locale (`server.py`) | testata con 3.11; stdlib pura |
| PyYAML | editor di configurazione da `admin.html` | opzionale, vedi sotto |

PyYAML serve **solo** all'editor di configurazione di `admin.html` (scrittura di
`config/conf.yaml` tramite `PUT`). Il portale in sé, il catalogo e l'anteprima dati non ne
hanno bisogno: il browser legge i file con un mini-parser YAML incluso in
`js/parsers.js`. Se installi PyYAML potrai usare anche l'editor di configurazione in locale.

## Clonare il repository

```bash
git clone https://github.com/pietrobiase/dataplana.git
cd dataplana
```

La struttura del repository:

```
dataplana/
├── src/               # il portale (tutto ciò che va pubblicato)
├── docs/              # questa documentazione (MkDocs / ReadTheDocs)
├── mkdocs.yml         # configurazione della documentazione
├── README.md
└── LICENSE            # AGPL-3.0
```

> Il progetto è tutto contenuto in `src/`: è una cartella di file statici, niente pipeline
> di build, niente `npm install`, niente bundler.

## Installare le dipendenze opzionali (PyYAML)

Solo se vuoi l'editor di configurazione in locale da `admin.html`:

```bash
cd src
python -m pip install pyyaml
```

Oppure, in alternativa, aggiorna `config/conf.yaml` a mano con un editor di testo: vedi
[Configurazione](configurazione.md).

## Verificare l'installazione

Dalla cartella `src/`:

```bash
python server.py
```

Poi apri <http://localhost:8000>. Vedi [Avvio rapido](avvio-rapido.md) per i dettagli.

## Deploy senza alcuna installazione

DataPlana è pensato per funzionare senza runtime lato server. Se hai già un hosting web
(anche solo statico), puoi saltare l'installazione di Python e caricare direttamente i file
di `src/`: il portale funziona identico; perderai soltanto il salvataggio del catalogo
dall'admin (che in produzione si aggira con la modalità "Esporta catalogo" + caricamento del
file `datasets.json` via FTP). Vedi [Deploy](deploy.md).

## Prossimi passi

- [Avvio rapido](avvio-rapido.md) — far girare il portale in locale
- [Configurazione](configurazione.md) — personalizzare il portale