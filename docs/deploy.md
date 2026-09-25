# Deploy

DataPlana è un portale **statico**: per pubblicarlo su un dominio basta caricare i file del
portale (`src/`) su un hosting web. Non c'è runtime da installare, non c'è database, non c'è
processo da tenere acceso. Questa pagina spiega come farlo, in particolare sull'hosting di
riferimento del progetto (Apache con supporto `.htaccess`), con una sezione per gli hosting
statici generici.

## Cosa pubblicare

Carica nella directory dell'hosting mappata sul percorso voluto (es.
`https://www.sovranita-digitale.it/dataset/`) **esattamente** questi elementi di `src/`:

```
index.html
admin.html
catalogo.html
privacy-policy.html
credits.html
css/
js/
config/
dataset/
fonts/
vendor/
components/
deploy/.htaccess   → rinominato ".htaccess" nella root del sito
deploy/.htpasswd   → nella posizione indicata dentro .htaccess
```

## Cosa NON caricare

**Non caricare mai**: `server.py`, `lancio.bat` e la cartella `local/`.
`server.py` è solo per lo sviluppo locale; il deploy è 100% statico.

## Configurare `.htaccess` e `.htpasswd` (hosting Apache)

Il progetto fornisce due file già pronti in `src/deploy/`:

- **`.htaccess`** — fornisce:
  - `Options -Indexes` (niente elenco directory);
  - header di sicurezza (`X-Frame-Options`, `X-Content-Type-Options`,
    `Referrer-Policy`);
  - **Basic Auth sulla sola `admin.html`**;
  - blocco dell'accesso via web a `.htaccess`, `.htpasswd` e file `.log`.

  Procedura:
  1. copia `deploy/.htaccess` nella root del sito e **rinominalo** in `.htaccess`;
  2. imposta **`AuthUserFile`** al percorso assoluto (filesystem) del file `.htpasswd`. In
     un hosting cPanel tipico, se il portale sta in `/dataset/`:

     ```
     AuthUserFile /home/UTENTE/public_html/sovranita-digitale.it/dataset/.htpasswd
     ```

  Il percorso **non deve essere un URL**: è il path sul disco del server.

- **`.htpasswd`** — contiene le credenziali dell'admin (formato `$6$`/SHA-512 crypt,
  compatibile con `htpasswd` di Apache). **Di default è `admin` / `admin`: cambialo subito**
  prima di pubblicare — vedi [Credenziali dell'area admin](credenziali.md). Caricalo nella
  posizione dichiarata in `AuthUserFile` permettendo solo le letture del web server (perms
  `640`, owner del web server).

### La coppia extra in `local/`

Puoi testare l'Apache reale in locale (XAMPP/MAMP) usando `local/.htaccess` e
`local/.htpasswd`, che hanno le stesse credenziali. Non fanno parte del deploy.

## Deploy su hosting statici generici

DataPlana gira su qualsiasi hosting statico (es. Netlify, GitHub Pages, Vercel, S3+CloudFront):
carica tutti i file di `src/` tranne lato `deploy/`/`local/` (niente `.htaccess`/`.htpasswd`
su hosting non-Apache). In questi casi:

- la **protezione di `admin.html`** con Basic Auth Apache **non è disponibile**: se carichi
  `admin.html` su un hosting senza autenticazione, chiunque potrà aprire l'interfaccia. La
  pubblicazione su hosting statici è quindi pensata per chi pubblica solo il portale e non
  l'admin; in alternativa proteggila con l'autenticazione dell'hosting (es. Netlify Identity)
  oppure non pubblicare `admin.html` e aggiorna il catalogo offline con **"Esporta
  catalogo"**;
- il **salvataggio del catalogo** dall'admin richiede WebDAV; altrimenti aggiorni
  `config/datasets.json` offline e lo ricarichi in `config/`.

Usa il workflow descritto in [Catalogo e amministrazione](amministrazione.md#workflow-di-aggiornamento-dei-dati).

## Verifica post-deploy

- `https://IL-TUO-DOMINIO/dataset/` → la home si carica, icone e font si vedono (nessuna
  richiesta esterna: controlla la scheda Network degli strumenti sviluppatore).
- `https://IL-TUO-DOMINIO/dataset/catalogo.html` → la ricerca e i filtri funzionano.
- `https://IL-TUO-DOMINIO/dataset/admin.html` → chiede le credenziali (default
  `admin`/`admin`, da cambiare: vedi [Credenziali dell'area admin](credenziali.md)).
- Le pagine Privacy Policy e Credits si aprono e rendono correttamente il Markdown.

## Note su HTTP/HTTPS

Sempre **HTTPS**: oltre a essere la norma per un portale pubblico, evita che il browser
blocchi i `fetch()` dei file di configurazione (mixed content) quando il portale viene
chiamato con `https://` e i file sarebbero serviti da `http://`.

## Evoluzione prevista

Una REST API pubblica (read) è prevista come evoluzione futura, non inclusa in questa
versione.

## Riferimenti

- [Avvio rapido](avvio-rapido.md) — provare il portale in locale prima del deploy
- [Catalogo e amministrazione](amministrazione.md) — aggiornare i dati in produzione
- [Configurazione](configurazione.md) — personalizzare titolo, SEO, colori