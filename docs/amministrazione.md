# Catalogo e amministrazione

Questa pagina spiega come gestire i **dataset** del portale: i metadati in
`config/datasets.json`, l'interfaccia `admin.html` e il workflow consigliato per aggiornare
i dati. La configurazione del portale (titolo, colori, SEO…) è trattata nella pagina
[Configurazione](configurazione.md).

## Dove vivono i dati

Il catalogo è il file **`config/datasets.json`**: una lista JSON di oggetti, uno per
dataset. Il browser lo legge via `fetch()` a ogni apertura del catalogo.

### Struttura di una voce del catalogo

```json
{
  "id": "comuni-italiani",
  "title": "Comuni italiani",
  "description": "Elenco dimostrativo dei comuni italiani.",
  "category": "Territorio",
  "publisher": "ISTAT",
  "license": "CC BY 4.0",
  "updated": "2026-09-20",
  "files": [
    { "format": "JSON", "file": "dataset/comuni-italiani.json" },
    { "format": "CSV", "file": "dataset/comuni-italiani.csv" }
  ],
  "tags": ["comuni", "territorio", "italia"]
}
```

| Campo | Tipo | Obbligatorio | Note |
|-------|------|--------------|------|
| `id` | stringa | sì | identificatore univoco; è l'ancora delle URL; **immutabile** dopo la creazione (disattivato nel form di modifica) |
| `title` | stringa | sì | titolo della scheda |
| `description` | stringa | no | descrizione mostrata nel catalogo |
| `category` | stringa | no | deve corrispondere a una `label` delle categorie in `conf.yaml` |
| `publisher` | stringa | sì | ente pubblicatore |
| `license` | stringa | no | nome della licenza (es. "CC BY 4.0") |
| `updated` | stringa | no | data ISO `YYYY-MM-DD` |
| `files` | array | sì | almeno un elemento `{format, file}`; `format` deve corrispondere a un formato configurato |
| `tags` | array di stringhe | no | liberi, usati dalla ricerca |

**Percorsi file**: devono essere relativi e interni al progetto (es. `dataset/nome.json`);
niente percorsi assoluti, niente `..`, niente `C:\…`.

## L'interfaccia admin (`admin.html`)

La pagina di amministrazione permette di:

- **Creare, modificare ed eliminare** voci del catalogo (form con validazione
  percorsi-file e controllo ID duplicati);
- **Esplorare i dati** di un dataset (anteprima in tabella delle **prime 200 righe** del
  file selezionato);
- **Configurare il portale** (`conf.yaml`) tramite l'editor messo a disposizione nel
  pannello "Configurazione" — vedi [Configurazione](configurazione.md);
- **Esportare il catalogo** (scarica `datasets.json` aggiornato da ricaricare in `config/`);
- **Uscire** con un logout dalla cache di Basic Auth del browser.

### Nota: eliminare una voce NON cancella i file

> Eliminare un dataset dal catalogo **non cancella** i file in `dataset/`: il catalogo è
> solo l'indice. Se vuoi rimuovere anche i dati, elimina i file via FTP/SSH.

## Credenziali

!!! danger "Credenziali di default: `admin` / `admin` — vanno CAMBIATE"

    Il progetto viene fornito con utente e password predefiniti:

    > **Utente:** `admin`
    >
    > **Password:** `admin`

    Sono pubblici (li trovi in questa documentazione) e chiunque può usarli per entrare nel
    pannello. **È obbligatorio cambiarli subito**, in locale e in produzione, prima di
    pubblicare il portale. Tutte le istruzioni — come generare un nuovo hash, come
    sostituirlo in `deploy/.htpasswd` e `local/.htpasswd`, anche tramite strumenti online —
    sono nel capitolo [Credenziali dell'area admin](credenziali.md).

Il nome utente e la password sono verificati contro `deploy/.htpasswd` (formato `$6$` =
SHA-512 crypt), compatibile con `htpasswd` di Apache per il deploy. In locale il `server.py`
usa lo stesso file. Esiste una copia identica in `local/.htpasswd` per chi testa con Apache
vero.

## Workflow di aggiornamento dei dati

1. **Carica i file** in `dataset/` (tramite FTP/SSH sul server, o copiandoli in locale):
   ogni dataset può avere più file (JSON, CSV, GeoJSON, XML, XLSX…).
2. **Apri `admin.html`** e aggiorna i **metadati**: nell'editor "File del dataset" aggiungi
   una riga per ogni formato (es. JSON e CSV dello stesso dataset), indicando il percorso
   relativo del file.
3. **Salva il catalogo**:
   - in locale, la modifica viene scritta in `config/datasets.json` dal `server.py`;
   - sul deploy, il salvataggio online richiede **WebDAV**; altrimenti usa il pulsante
     **"Esporta catalogo"** e ricarica `datasets.json` via FTP in `config/`.

## Note di sicurezza

- `server.py` è solo per sviluppo locale; `python -m http.server` non consentirebbe il
  salvataggio del catalogo.
- Le password in `.htpasswd` sono hash SHA-512 crypt (non in chiaro).
- Il progetto è intenzionalmente senza database: i file JSON/YAML sono lo storage.
- In produzione l'accesso ad `admin.html` è protetto da Basic Auth configurata nel
  `.htaccess` — vedi [Deploy](deploy.md).