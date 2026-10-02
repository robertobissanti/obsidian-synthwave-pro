# TODO: Synthwave Pro

Piano delle prossime modifiche.

**Stato (2026-10-02):** R-0…R-5, "Compact headings" e le pulizie del README sono implementati su questo branch, non ancora rilasciati (`manifest.json` resta `1.0.1`). Prima del rilascio vanno provati in Obsidian i punti segnati "da verificare" qui sotto, in particolare:

- [ ] lo slider Font size di Obsidian e Ctrl + rotella cambiano il testo; "Override Obsidian font size" lo sostituisce;
- [ ] Style Settings scrive le variabili solo per i valori cambiati (altrimenti "Use custom colors" sovrascriverebbe tutta la palette con i default Synthwave);
- [ ] in Live Preview i `#` degli heading compatti compaiono una sola volta, sia sulla riga col cursore sia sulle altre;
- [ ] le scanlines non coprono il visualizzatore immagini su mobile né i lightbox dei plugin; un'immagine aperta in una scheda propria ha ancora le scanlines (non risolto);
- [ ] stili dei callout e heading compatti in tutte le palette, dark e light.
- [ ] stile di testo Terminal: in Reading view e Live Preview ogni blocco parte su una riga della griglia (testo, liste, codice, callout, tabelle, linee orizzontali); le immagini restano l'unica eccezione nota.

**Riordino Style Settings (2026-10-02):** sezioni Palette, Typography, Headings, Images, Effects, Callouts, Advanced. Rimosse le opzioni font dell'interfaccia, font degli heading, dimensione del codice, spaziatura paragrafi, spessore bordo callout, colori di grassetto e corsivo, dimensioni H1…H6 (sostituite da un unico "Heading size"). "Monospace headings" è diventato "Text style" (Classic = come prima, Clean, Terminal). Chi aveva spento "Monospace headings" torna a Classic: da segnalare nelle note di rilascio.
Versione corrente: `1.0.1`. Versione prevista per questo blocco di lavoro: **`1.1.0`** (nuove funzioni, nessuna rottura delle impostazioni esistenti).

## 1. Richieste degli utenti dal thread Reddit

Fonte: https://www.reddit.com/r/ObsidianMD/s/PBG2w1IGQx (commenti di u/Key-Concept-7001; u/rob-bix è l'autore del tema). Tutte le opzioni nuove vanno in Style Settings e sono spente o neutre di default, così chi usa già il tema non vede cambiamenti.

### R-0 Bug: lo slider della dimensione del font di Obsidian non ha effetto

> "The only problem I'm seeing is that the font size doesn't change with the slider" (u/cinematic_j)

Da correggere per primo, anche come patch `1.0.2` se il resto della 1.1.0 tarda.

Causa probabile, trovata leggendo `theme.css` (da confermare in Obsidian):
- in `body` (sez. 1) il tema imposta `--font-text-size: var(--swp-font-size)`, cioè lega la dimensione al proprio slider Style Settings (default 16px);
- in sez. 5 imposta `font-size: var(--swp-font-size)` direttamente su `.markdown-preview-view`, `.markdown-source-view` e `.cm-editor`. Questa regola ignora `--font-text-size`, quindi anche se Obsidian aggiorna la sua variabile quando si muove lo slider in Settings → Appearance → Font size (e con Ctrl + rotella), il testo resta fermo a `--swp-font-size`.

Correzione prevista:
- [ ] Togliere `--font-text-size: var(--swp-font-size)` da `body` e le dichiarazioni `font-size: var(--swp-font-size)` della sez. 5, così comanda lo slider di Obsidian.
- [ ] Lo slider "Base font size" di Style Settings diventa facoltativo: `class-toggle` `swp-override-font-size` (default off), e solo con quella classe si applica `--font-text-size: var(--swp-font-size)`. Va detto nella descrizione dell'opzione che sostituisce lo slider di Obsidian.
- [ ] Controllare che `line-height` e `letter-spacing` della sez. 5 restino validi.
- [ ] Provare: slider di Obsidian, Ctrl + rotella, Ctrl + `+`/`-` (zoom dell'interfaccia), con e senza Style Settings installato.

### R-1 Più controllo sulla tipografia

> "I like to customize typography in a theme, including the font size."

Oggi esistono già font del testo, font mono, dimensione base e interlinea, ma stanno in una sezione chiusa chiamata "Heading section", che sembra riguardare gli heading. Probabile che l'utente non le abbia trovate.

- [ ] Rinominare il titolo della sezione in "Typography" (solo `title`, l'id `swp-fonts-heading` resta uguale per non perdere le impostazioni) e aprirla di default (`collapsed: false`).
- [ ] Aggiungere `variable-text` per il font dell'interfaccia (`--font-interface-theme`, oggi uguale al testo) e per il font degli heading (`--swp-font-heading`, usato anche da "Monospace headings").
- [ ] Aggiungere slider per: dimensione di ciascun heading h1-h6 in `em` (mappati su `--h1-size` … `--h6-size`), peso del testo (`--font-weight`), peso del grassetto (`--bold-weight`), dimensione del codice (`--code-size`), larghezza di lettura (`--file-line-width`), spaziatura tra paragrafi (`--p-spacing`), dimensione dell'interfaccia (`--font-ui-small` / `--font-ui-medium`).
- [ ] Partire dopo la correzione di R-0: le nuove impostazioni non devono di nuovo scavalcare lo slider di Obsidian.
- [ ] Controllare che gli slider degli heading non entrino in conflitto con "Compact headings" (sez. 2): con l'opzione attiva le dimensioni restano a `1em`.

### R-2 Attenuare le immagini (image dimming)

> "Some themes let you dim images. Since you find screen comfort important it's worth adding."

- [ ] `class-toggle` `swp-dim-images` (default off) e slider `swp-image-dim` (luminosità, default 80%, range 50-100%).
- [ ] CSS, solo in dark mode:
  ```css
  body.theme-dark.swp-dim-images :is(.markdown-rendered, .markdown-source-view) img {
    filter: brightness(var(--swp-image-dim, 80%));
    transition: filter .2s;
  }
  body.theme-dark.swp-dim-images :is(.markdown-rendered, .markdown-source-view) img:hover {
    filter: none;
  }
  ```
- [ ] Opzione facoltativa: attenuare anche i video e gli embed PDF.
- [ ] Il ripristino al passaggio del mouse non esiste su mobile: verificare che l'immagine aperta a schermo intero non resti scura.

### R-3 Colori scelti dall'utente sopra la palette

> "Let the user choose their own colors, overriding the palette choice. They can start with a palette and then override individual elements if they want to."

Bug da sistemare insieme: le impostazioni "Override primary accent" e "Override secondary accent" esistono già nel blocco `@settings`, ma nessuna regola CSS usa `--swp-accent-primary` o `--swp-accent-secondary`, quindi oggi non hanno alcun effetto.

- [ ] Separare i valori della palette da quelli effettivi. Le palette definiscono `--swp-p-accent-1`, `--swp-p-bg-1` ecc., e il tema usa `--swp-accent-1: var(--swp-c-accent-1, var(--swp-p-accent-1))`. Se l'utente imposta un colore, Style Settings scrive `--swp-c-…` e vince; se lo azzera, torna quello della palette.
- [ ] Verificare se Style Settings scrive la variabile anche quando il valore è quello di default. Se sì, serve un `class-toggle` "Use custom colors" e le regole di override valgono solo con quella classe.
- [ ] Elementi da rendere personalizzabili (`variable-themed-color`, dark e light separati): 4 accenti, sfondo editor, sfondo sidebar, testo, testo attenuato, link, tag, evidenziazione, colori h1-h6, grassetto, corsivo, codice inline.
- [ ] Sostituire le due impostazioni esistenti con le nuove, mantenendo gli id `swp-accent-primary` e `swp-accent-secondary` per chi li ha già impostati.
- [ ] Nel README spiegare il flusso: scegli una palette, poi cambia solo i colori che vuoi.

### R-4 CRT scanlines spente sulle immagini a schermo intero

> "When CRT scan lines is on, consider switching that off when the image is full screen after clicking or tapping."

Oggi le scanlines sono un `body::after` fisso con `z-index: 9999`, quindi stanno sopra tutto, compresi modali e lightbox delle immagini.

- [ ] Spostare l'overlay dal `body` a `.workspace` (o `.app-container`). Modali, lightbox dei plugin e visualizzatore immagini su mobile vengono aggiunti al `body` fuori da `.workspace`, quindi finiscono sopra le scanlines.
- [ ] Per un'immagine aperta in una scheda propria (`.workspace-leaf-content[data-type="image"]`), applicare le scanlines per singola scheda (`.workspace-leaf-content:not([data-type="image"])::after` con `position: absolute`) invece che su tutta la finestra. Valutare se questa resa per scheda è da preferire in ogni caso.
- [ ] Provare con il visualizzatore immagini di Obsidian mobile e con i plugin di zoom più diffusi (Image Toolkit, Mousewheel Image Zoom).
- [ ] Niente `:has()` (sconsigliato dalle linee guida Obsidian).

### R-5 Più opzioni di design per i callout

> "I use callouts a lot. A few design options would make the theme more interesting."

Oggi i callout hanno solo raggio di 8px, bordo al 40% del colore e due tipi ridisegnati (IMPORTANT, CAUTION).

- [ ] `class-select` "Callout style":
  - Default (attuale)
  - Flat (niente bordo, solo sfondo)
  - Left bar (barra spessa a sinistra, sfondo leggero)
  - Neon (bordo con `box-shadow` del colore del callout, coerente con "Neon glow")
  - Terminal (font mono, titolo in maiuscolo con prefisso `>`)
- [ ] Slider per raggio degli angoli, intensità dello sfondo (`color-mix` con `rgb(var(--callout-color))`) e spessore del bordo.
- [ ] Toggle "Callout colors from palette": mappa i tipi principali (note, tip, warning, danger, quote…) sugli accenti `--swp-accent-*` invece dei colori standard di Obsidian.
- [ ] Opzioni per il titolo: grassetto o normale, maiuscolo, icona nascosta.
- [ ] Controllare contrasto e leggibilità in tutte le palette, dark e light, anche per i callout ripiegabili e annidati.

## 2. Nuova opzione: heading compatti ("compact headings")

Idea ispirata a https://stephan.zych.be/. Gli header markdown hanno la stessa dimensione del testo, mantengono il colore del livello (`--h1-color` … `--h6-color`), restano in grassetto, mostrano i segni `#` iniziali e hanno una banda di sfondo di colore diverso su tutta la riga.

### 2.1 Impostazioni Style Settings

Da aggiungere nel blocco `@settings` di `theme.css`, in una nuova sezione dopo "Monospace headings":

```yaml
  -
    id: swp-headings-section
    title: Compact headings
    type: heading
    level: 3
    collapsed: true

  -
    id: swp-compact-headings
    title: Compact headings
    description: Headings use the body font size, stay bold and colored, keep their leading # marks and get a background band.
    type: class-toggle
    default: false

  -
    id: swp-heading-band
    title: Heading band color
    description: How the background band behind compact headings is colored.
    type: class-select
    default: swp-band-tinted
    allowEmpty: false
    options:
      -
        label: Tinted by heading level (default)
        value: swp-band-tinted
      -
        label: Single custom color
        value: swp-band-custom
      -
        label: No band
        value: swp-band-none

  -
    id: swp-heading-band-strength
    title: Band intensity (%)
    description: How much of the heading color is mixed into the band (tinted mode).
    type: variable-number-slider
    default: 14
    format: '{}%'
    min: 4
    max: 40
    step: 1

  -
    id: swp-heading-band-color
    title: Custom band color
    description: Used when "Single custom color" is selected.
    type: variable-themed-color
    format: hex
    default-dark: '#2a1f4a'
    default-light: '#ece6fb'
    opacity: false
```

Gli id sono nuovi e non toccano quelli esistenti, quindi gli utenti attuali non perdono le loro impostazioni.

### 2.2 Token CSS

In `body` (sez. 1) aggiungere i default delle variabili, così il tema funziona anche senza Style Settings:

```css
body {
  --swp-heading-band-strength: 14%;
  --swp-heading-band-color: var(--swp-bg-2);
}
```

Con l'opzione attiva si usano le variabili ufficiali di Obsidian per dimensione, peso e interlinea, così valgono sia nell'editor sia nella lettura:

```css
body.swp-compact-headings {
  --h1-size: 1em; --h2-size: 1em; --h3-size: 1em;
  --h4-size: 1em; --h5-size: 1em; --h6-size: 1em;
  --h1-weight: 700; --h2-weight: 700; --h3-weight: 700;
  --h4-weight: 700; --h5-weight: 700; --h6-weight: 700;
  --h1-line-height: var(--swp-line-height);
  /* idem per h2…h6 */
}

/* colore della banda per livello */
body.swp-compact-headings.swp-band-tinted {
  --swp-band-h1: color-mix(in srgb, var(--h1-color) var(--swp-heading-band-strength), var(--background-primary));
  /* idem per h2…h6 con --h2-color … --h6-color */
}
body.swp-compact-headings.swp-band-custom {
  --swp-band-h1: var(--swp-heading-band-color);
  /* idem per h2…h6 */
}
body.swp-compact-headings.swp-band-none {
  --swp-band-h1: transparent;
  /* idem per h2…h6 */
}
```

Da verificare: `--hN-size` / `--hN-weight` / `--hN-line-height` sono le variabili documentate da Obsidian per gli heading; se in una versione di Obsidian non bastano, aggiungere `font-size: var(--font-text-size)` direttamente sui selettori sotto.

### 2.3 Editor: Live Preview e Source mode

Ogni riga di heading in CodeMirror è `.cm-line.HyperMD-header.HyperMD-header-N`, quindi la banda va su quella riga:

```css
body.swp-compact-headings .markdown-source-view .HyperMD-header-1 {
  background-color: var(--swp-band-h1);
  border-radius: 4px;
  padding-inline: 0.5em;   /* padding, non margin: le linee guida Obsidian vietano margini verticali in live preview */
}
/* idem per 2…6 */
```

Segni `#`:

- **Source mode**: i `#` sono già nel testo (`.cm-formatting-header`). Basta dargli il colore del livello attenuato: `color: color-mix(in srgb, var(--hN-color) 55%, transparent)`.
- **Live Preview**: Obsidian nasconde i `#` quando il cursore non è sulla riga, e li mostra sulla riga attiva (`.cm-active`). Per averli sempre visibili senza doppioni:
  ```css
  body.swp-compact-headings .is-live-preview .HyperMD-header-1:not(.cm-active)::before {
    content: "# ";
    color: color-mix(in srgb, var(--h1-color) 55%, transparent);
  }
  ```
  Così sulla riga attiva si vedono i `#` reali, sulle altre quelli del `::before`. Niente `:has()` (sconsigliato dalle linee guida).
- Da verificare in devtools: se i `#` nascosti restano nel DOM come span con `display: none`, preferire rimostrarli con CSS invece del `::before`. Controllare anche selezioni su più righe, il fold indicator (`.cm-fold-indicator`) che si sposta con il padding, e i link interni dentro l'heading.

Banda a tutta larghezza: di default la banda segue la larghezza della riga (che con "Readable line length" è quella del testo). Una variante a tutto schermo si può fare con `box-shadow: 0 0 0 100vmax var(--swp-band-hN); clip-path: inset(0 -100vmax);`, ma va provata con scroll orizzontale, tabelle larghe e split pane. Proposta: rimandare a dopo, partendo dalla banda sulla riga.

### 2.4 Reading view

```css
body.swp-compact-headings .markdown-rendered h1 {
  background-color: var(--swp-band-h1);
  border-radius: 4px;
  padding: 0.15em 0.5em;
}
body.swp-compact-headings .markdown-rendered h1::before {
  content: "# ";
  color: color-mix(in srgb, var(--h1-color) 55%, transparent);
}
/* idem per h2…h6 con ##, ###, ####, #####, ###### */
```

Note:

- Oggi i `#` in lettura esistono solo con "Monospace headings" e solo per h1-h4 (`theme.css`, sez. 5). Unificare: una sola regola `::before` valida se è attiva almeno una delle due opzioni, ed estenderla a h5 e h6. Così non ci sono doppi `#` quando entrambe le opzioni sono attive.
- Escludere l'inline title (`.inline-title`), che deve restare grande.
- Controllare heading dentro callout, embed (`.markdown-embed`), hover preview e export PDF (`@media print`): la banda colorata in stampa potrebbe non servire.
- Con "Neon glow" attivo il `text-shadow` resta valido; verificare che sulla banda non sia troppo.
- Controllare il contrasto (WCAG AA) del testo sulla banda in tutte e 5 le palette, dark e light, soprattutto `Soft Reading` e le palette light.

### 2.5 Documentazione

- [ ] `README.md` e `README_it.md`: nuova voce "Compact headings" tra le opzioni, con uno screenshot.
- [ ] Facoltativo: screenshot aggiuntivo per il listing su community.obsidian.md ("Edit listing").

## 3. Pulizie già note (dallo studio del 2026-10-02)

Da `/notes/studio-tema-e-regole-obsidian.md` nei file del progetto:

- [ ] README: mettere per prima l'installazione da Settings → Appearance → Themes → Manage ("Synthwave Pro"); la copia manuale va dopo.
- [ ] README: aggiornare la sezione "Structure" con i file reali.
- [ ] README: indicare la versione minima dell'installer di Obsidian, perché il tema usa `color-mix()` (Chromium 111+).
- [ ] Facoltativo: workflow GitHub Actions `.github/workflows/release.yml` che crea la release al push del tag.

## 4. Rilascio

1. Implementare e provare le voci sopra in un vault di test (dark e light, tutte le palette, Live Preview, Source mode, Reading view).
2. Incrementare `version` in `manifest.json` a `1.1.0`. `minAppVersion` resta `1.4.0` salvo nuove dipendenze.
3. Commit e push su `main`: Obsidian legge il manifest dall'HEAD del branch di default.
4. Creare il tag **identico** alla versione, senza `v`: `git tag -a 1.1.0 -m "1.1.0"` e `git push origin 1.1.0`.
5. Creare la release GitHub con tag `1.1.0` e allegare `manifest.json` e `theme.css`.
6. Su community.obsidian.md, pagina del tema: "Check for new releases" e controllare l'esito della review automatica.
7. `versions.json` non serve ai temi.
8. Rispondere sul thread Reddit (u/Key-Concept-7001) con le novità.

## Correzioni dopo il primo test in Obsidian (2026-10-02)

- [x] Callout senza sfondo né bordo: da Obsidian 1.13 `--callout-color` è un colore completo, non più una terna `r, g, b`. Il tema usava `rgb(var(--callout-color))` (anche nella 1.0.1, per il bordo), che ora è invalido. Ora usa `var(--callout-color)`; IMPORTANT e CAUTION hanno colori esadecimali.
- [x] Terminal: testo dei callout (e di altri widget in Live Preview) proporzionale. Obsidian imposta `font-family: var(--font-text)` su `.cm-scroller` e sui widget; ora Terminal ridefinisce `--font-text` sulle viste.
- [x] Heading compatti nell'editor più alti di una riga e disallineati dai numeri di riga: Obsidian aggiunge `padding-top: var(--p-spacing)` alle righe heading, finito dentro la banda. Azzerato con Compact headings.
- [x] Titolo della nota (inline title) proporzionale: in Classic e Terminal ora usa il font monospace come gli heading.
- [ ] Verificare se serve alzare `minAppVersion` alla versione di Obsidian che ha cambiato il formato di `--callout-color`.

## Palette Catppuccin (2026-10-02)

- [x] Aggiunte Catppuccin Frappé, Macchiato e Mocha (in light mode tutte e tre usano Latte), colori ufficiali da `catppuccin/palette` (`palette.json`). Mappatura: crust/base/mantle/surface0 per gli sfondi, surface1 per le linee, text/subtext0/overlay0 per il testo, mauve/blue/pink/yellow come accenti, green/peach/red per successo/avviso/errore. Credito MIT nei README.
- [x] Background depth ora usa i toni della palette attiva: prima Deep e Soft in dark mode avevano valori fissi di Synthwave e davano sfondi sbagliati alle altre palette.

## Audit delle opzioni (2026-10-02)

- [x] Le opzioni rese inutili da un'altra scelta sono nascoste nel pannello Style Settings (CSS su `.setting-item[data-id]`, classi sul body aggiornate da Style Settings al clic). Tabella delle regole nella sezione 9c di `theme.css`.
- [x] "Bold weight" non aveva effetto: Obsidian 1.13 disegna il grassetto con `--bold-modifier`, non `--bold-weight`. Nuovo id `swp-bold-weight` che imposta il modificatore.
- [x] Testo di "Text font" chiarito per Terminal (vale solo per l'interfaccia); stile callout "Terminal" rinominato "Dashed monospace" per non confonderlo con lo stile di testo.
- [x] Slider con unità (Corner radius, Base font size, Readable line width, Band intensity, Image brightness) producevano valori invalidi: Style Settings aggiunge `format` alla lettera dopo il numero, quindi `'{}px'` dava `8{}px`. Ora `format: px` / `'%'`. Il difetto c'era già nella 1.0.1 per Base font size.
