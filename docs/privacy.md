# Privacy policy

Il portale include una pagina **Privacy Policy** servita come file statico e quindi
configurabile senza ricompilare nulla: il contenuto vive in **`config/privacy-policy.md`**
(un file Markdown) e viene reso un'HTML al volo nel browser.

## Come funziona

- La pagina è **`privacy-policy.html`**: scarica `config/privacy-policy.md`, lo interpreta
  con il renderer Markdown di `js/parsers.js` (`renderMarkdown`) e ne mostra il contenuto
  formattato.
- Il link del footer **"Privacy Policy"** è definito in `conf.yaml` (voce `links`) e punta a
  `privacy-policy.html` — vedi [Configurazione](configurazione.md#links-lista-di-oggetti-title-url).

Funziona quindi in due punti:

1. modifica il contenuto in `config/privacy-policy.md`;
2. ricarica la pagina `privacy-policy.html` (o il portale) per vedere l'effetto.

## Configurare il contenuto

Il file di partenza è un esempio già strutturato secondo la normativa italiana:

```markdown
# Privacy Policy

Ultimo aggiornamento: **23 settembre 2026**

## 1. Titolare del trattamento
...

## 2. Dati raccolti
...

## 3. Finalità e base giuridica
...
```

Il renderer supporta: titoli, elenchi (ordinati e puntati), tabelle, citazioni (`>`),
codice inline e a blocchi, link e testo in grassetto/corsivo. Puoi riscrivere il file a
piacere, mantenendo le sezioni che servono al tuo progetto (un punto di partenza tipico:
Titolare del trattamento, Dati raccolti, Finalità e base giuridica, Conservazione, Diritti
dell'interessato, Modifiche).

> Consiglio: indica sempre la **data di ultimo aggiornamento** in cima al documento, come
> nell'esempio fornito, così resta esaustiva nel tempo.

## Note sulla natura statica del portale

DataPlana **non installa tracker, cookie di profilazione o librerie esterne**: font e icone
sono serviti localmente e il portale non effettua richieste a CDN o servizi di terze parti.
Questo si riflette nella sezione "Dati raccolti" della privacy policy, che in un
portale demo non ha nulla di strumentale da dichiarare. Se in futuro aggiungessi analisi o
cookie, ricordati di aggiornare il file di conseguenza.

## Verifica

Apri <http://localhost:8000/privacy-policy.html> e controlla che la resa sia corretta.
Ricorda sempre di servire le pagine tramite il web server (mai da `file://`).

## Argomenti correlati

- [Configurazione](configurazione.md) — dove definire il link nel footer e la posizione del
  file Markdown
- [Struttura del progetto](struttura.md) — collocazione dei file `config/`