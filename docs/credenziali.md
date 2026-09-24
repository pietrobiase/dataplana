# Credenziali dell'area admin

Le credenziali di accesso alla pagina **`admin.html`** (il pannello di amministrazione) sono
gestite tramite il file **`.htpasswd`** in formato Apache `$6$` (SHA-512 crypt). Il progetto
contiene **due copie**:

| File | Dove viene usato |
|------|------------------|
| `src/deploy/.htpasswd` | **Produzione** (hosting Apache) e **locale** con `server.py` |
| `src/local/.htpasswd` | Apache in locale (XAMPP/MAMP) solo per test |

!!! danger "LE CREDENZIALI DI DEFAULT SONO `admin` / `admin`"

    Il repository viene fornito con utente e password predefiniti pari a:

    > **Utente:** `admin`
    >
    > **Password:** `admin`

    **Questo NON è un accesso sicuro.** Chiunque conosca il progetto (o apra questa
    documentazione) potrà entrare nel pannello di amministrazione: basterà indovinare
    l'esistenza della coppia `admin`/`admin` per accedere al catalogo e alla configurazione
    del portale.

    **È OBBLIGATORIO cambiare almeno la password PRIMA di ogni utilizzo**, sia in locale sia
    in deploy. Le istruzioni sono in fondo a questa pagina.

!!! warning "Controllo necessario anche dopo aver pubblicato"

    Se hai già fatto il deploy con le credenziali di default, considera l'username
    **già compromesso**: cambiando solo la password puoi continuare a usare `admin` come
    nome utente, ma è consigliabile usare un **nome utente non prevedibile** (niente
    `admin`, `root`, `hq`, `utente`, ecc.).

!!! note "Il server `server.py` legge sempre `deploy/.htpasswd`"

    In sviluppo locale l'autenticazione dell'admin è eseguita da `server.py`, che legge
    **sempre** `src/deploy/.htpasswd` (non `local/.htpasswd`). Per cambiare le credenziali in
    locale modifica quindi `deploy/.htpasswd`. La copia in `local/.htpasswd` serve solo se
    testi con un Apache vero (XAMPP/MAMP). In produzione si modifica lo stesso file
    `deploy/.htpasswd`, ma **caricato sul server** (vedi [Deploy](deploy.md)).

## Formato del file

Ogni riga del file `.htpasswd` contiene una coppia `utente:hash`:

```
admin:$6$wH0M972jfhvPfbJ7$ss0B./MDr4eakF2F92dgdUEREz1AfXGJz6C9lZ38r5KH76I1a/R.l4FSGRIN7eOBvXfpOAHT2oYrRXligkz4J/
```

- **`admin`** è il nome utente;
- dopo i due punti c'è **solo l'hash** della password (all'inizio `$6$` = algoritmo
  SHA-512-crypt): la password **non è mai salvata in chiaro**.

Il file può contenere più righe (più utenti); per ognuna si può accedere all'admin.
Il `server.py` (locale) e Apache (deploy) supportano esclusivamente il formato `$6$`.

## Come cambiare le credenziali

### Passo 1 — generare la nuova coppia (hash)

Crea un hash SHA-512-crypt `$6$` dalla password desiderata, con uno di questi metodi.

**Metodo A — strumento online (semplice, attenzione ai dati):**

1. Apri un generatore di hash `.htpasswd` online (ad esempio
   <https://hostingfilm.com/htpasswd-generator/> o
   <https://www.askapache.com/online-tools/htpasswd-generator/>).
2. Scegli l'algoritmo **SHA-512 (`$6$`)**.
3. Inserisci l'**username** e la **password** che hai scelto e genera.
4. Copia la riga `utente:$6$...` restituita.

!!! warning "Privacy: attenzione agli strumenti online"

    Inserire una password su un sito web la fa viaggiare su Internet e la espone a chi gestisce
    quel sito. Se possibile preferisci i metodi B o C (offline). In ogni caso **non usare
    mai** per il pannello la stessa password che usi per altri servizi (email, banche, ecc.).

**Metodo B — `htpasswd` di Apache/libreria (solo se hai Apache):**

```bash
htpasswd -bn -B nuovoutente nuovaPassword
```

!!! warning "Formato generato"

    Lo `htpasswd` di Apache oggi genera normalmente hash **bcrypt (`$2y$`)**, SHA-256
    (`$5$`) o MD5 (`$apr1$`). DataPlana accetta **solo** il formato `$6$` (SHA-512-crypt):
    se il tuo tool produce un formato diverso, usa uno degli altri metodi o uno strumento
    online con algoritmo **SHA-512 (`$6$`)**.

**Metodo C — Python (offline, consigliato):** il progetto include già la funzione
`sha512_crypt` in `server.py`, quindi puoi generare l'hash con un piccolo script senza
installare nulla:

```python
# gen_htpasswd.py  — esegui con: python gen_htpasswd.py
import getpass
import secrets
import string
import sys

sys.path.insert(0, r"CAMMINO/ABSOLUTO/PER/dataplana/src")
import server  # riusa la funzione sha512_crypt già presente nel progetto

utente = input("Username: ").strip()
password = getpass.getpass("Password: ")
salt = "".join(secrets.choice(string.ascii_letters + string.digits + "./") for _ in range(16))
print("%s:%s" % (utente, server.sha512_crypt(password, "$6$" + salt)))
```

### Passo 2 — sostituire la riga nei file

Con un editor (es. Notepad++) modifica il file `.htpasswd` interessato:

1. **Locale (con `server.py`):** apri `src/deploy/.htpasswd`.
2. **Deploy (produzione Apache):** apri `.htpasswd` **sul server** (lo stesso usato da
   `AuthUserFile` nel [`.htaccess`](deploy.md)), oppure scaricalo, modificalo e ricaricalo
   via FTP/SSH nella stessa posizione.
3. **Solo se usi Apache in locale (XAMPP/MAMP):** modifica anche `src/local/.htpasswd`.

Sostituisci la riga esistente con quella nuova (o aggiungi una nuova riga se vuoi aggiungere
un utente, lasciando le altre). Consigli:

- lascia invariato l'**username** se vuoi solo cambiare la password (`admin` o il nome che
  avevi usato);
- per un utente **nuovo** aggiungi una riga, non sovrascrivere;
- conserva l'**ultimo carattere newline** alla fine del file (nessun contenuto extra);
- la password **non deve contenere** i due punti `:` o nuovi a capo;
- dopo la modifica **ricarica la pagina** del browser: se la sessione precedente è rimasta in
  cache, fai logout dall'admin (pulsante "Uscita") o usa una finestra in incognito.

### Passo 3 — verificare

1. Avvia il server locale: `python server.py` (vedi [Avvio rapido](avvio-rapido.md)).
2. Apri <http://localhost:8000/admin.html>.
3. Il browser chiede le credenziali: inserisci **utente** e **password nuove**.
4. Accedi: se l'accesso riesce, la modifica è andata a buon fine. Prova anche a inserire una
   password errata per confermare che il pannello resta chiuso.

!!! success "Dopo la verifica"

    Ora il pannello è protetto e **tu conosci** le credenziali. Annota la nuova coppia nel
    memo locale `credenziali.txt` (file da NON pubblicare) oppure in un gestore di password.

## Creare più utenti

Per concedere l'accesso ad altre persone:

1. genera più hash con uno dei metodi sopra (Metodo B o C, non serve `-c`);
2. aggiungi nel `.htpasswd` una riga per ogni utente;

3. ricarica/upload del file e verifica come al Passo 3. Apache e `server.py` accettano
   qualsiasi utente presente nel file: tutti avranno gli stessi diritti (il pannello non
   distingue ruoli).

!!! warning "Più utenti = più responsabilità"

    Ogni utente aggiunto al `.htpasswd` può modificare catalogo e configurazione.
    Aggiungi solo persone di cui ti fidi e rimuovi tempestivamente le righe di chi non deve
    più accedere (cancellando la riga e ricaricando il file).

## Promemoria di sicurezza

- **Cambia subito** le credenziali di default `admin`/`admin` prima di pubblicare il portale.
- Non usare password deboli o già usate altrove; preferisci password lunghe generate da un
  gestore.
- Il file `.htpasswd` **non va mai** condiviso via web: il `.htaccess` incluso lo blocca già,
  e non va caricato `credenziali.txt` — vedi [Deploy](deploy.md#cosa-non-caricare).
- Se sospetti che le credenziali siano trapelate, cambiale subito.

## Riferimenti

- [Catalogo e amministrazione](amministrazione.md) — come usare l'admin
- [Deploy](deploy.md) — posizione di `.htpasswd` in produzione e `AuthUserFile`
- [Struttura del progetto](struttura.md) — disposizione dei file `deploy/` e `local/`