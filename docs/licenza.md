# Licenza

DataPlana è distribuito sotto **GNU Affero General Public License v3.0 (AGPL-3.0)**. Il testo
integrale è nel file [`LICENSE`](https://github.com/pietrobiase/dataplana/blob/main/LICENSE)
del repository.

## Cosa significa AGPL-3.0

- **Software libero**: puoi usare, studiare, modificare e ridistribuire DataPlana.
- **Obblighi**: se distribuisci una versione modificata, devi pubblicarne il sorgente sotto
  la stessa licenza (AGPL-3.0) e conservare le dichiarazioni di copyright.
- **Rete**: la clausola "network use" dell'AGPL estende l'obbligo di pubblicare il sorgente
  anche quando il software viene eseguito e offerto ad altri via rete (non solo quando si
  distribuisce un binario). Per un portale web, chi espone il portale deve rendere
  disponibile il sorgente che vi gira sopra.
- **Nessuna garanzia**: il software è fornito "così com'è", senza garanzie.

Il testo ufficiale è disponibile su [gnu.org/licenses/agpl-3.0.html](https://www.gnu.org/licenses/agpl-3.0.html).

## Licenze delle risorse incluse

Oltre al codice sorgente, DataPlana include risorse di terze parti, ciascuna con la propria
licenza (riportata anche nella pagina Credits del portale):

| Risorsa | Licenza | Posizione nel progetto |
|---------|---------|------------------------|
| Titillium Web (font) | SIL OFL 1.1 | `src/fonts/titillium-web/` |
| Font Awesome Free 6.5.2 (icone, CSS, webfont) | CC BY 4.0 · OFL 1.1 · MIT | `src/vendor/font-awesome/` |
| Dataset dimostrativi (es. ISTAT, ARPA) | CC BY 4.0 | `src/dataset/` |
| Codice sorgente (HTML, CSS, JS, Python) | AGPL-3.0 | `src/` |

### Titillium Web

Il font raccomandato dalle Linee Guida di design per i servizi web della PA (Docs Italia),
scaricato e ospitato localmente in `src/fonts/titillium-web/`. Licenza: SIL Open Font
License 1.1 — [scripts.sil.org/OFL](https://scripts.sil.org/OFL).

### Font Awesome Free 6.5.2

Icone per categorie e formati, servite localmente da
`src/vendor/font-awesome/css/all.min.css` (caricate solo se `fontAwesome` è valorizzato in
`config/conf.yaml`). Licenza: icone CC BY 4.0, font OFL 1.1, codice MIT —
[fontawesome.com/license/free](https://fontawesome.com/license/free).

### Dataset dimostrativi

I dataset di esempio sono puramente dimostrativi: ISTAT, ARPA e Comune demo sono
riferimenti indicativi. Licenza: CC BY 4.0 —
[creativecommons.org/licenses/by/4.0](https://creativecommons.org/licenses/by/4.0/deed.it).

## Attribuzione

Progettato e sviluppato da **Pietro Biase** ([pietrobiase.it](https://www.pietrobiase.it)).

## Dove trovare i crediti in pratica

La pagina **Credits** del portale (`credits.html`) mostra automaticamente tutte le
risorse, licenze e crediti, incluse le schede personalizzabili in `conf.yaml`
(crediti definiti dall'utente). Vedi [Configurazione](configurazione.md).