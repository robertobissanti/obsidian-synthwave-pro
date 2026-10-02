# TODO: Synthwave Pro

Piano delle prossime modifiche. Nulla di quanto segue è ancora implementato.
Versione corrente: `1.0.1`. Versione prevista per questo blocco di lavoro: **`1.1.0`** (nuove funzioni, nessuna rottura delle impostazioni esistenti).

## 1. Richieste degli utenti dal thread Reddit

Fonte: https://www.reddit.com/r/ObsidianMD/s/PBG2w1IGQx

> Da completare. Il thread non è stato leggibile dall'ambiente in cui è stato scritto questo piano (Reddit blocca l'accesso). Le issue GitHub del repo sono a zero, quindi non ci sono altre richieste registrate.

Per ogni commento del thread aggiungere una voce con questo formato:

- [ ] **R-n — titolo breve** (utente u/…, link al commento)
  - Richiesta: cosa chiede, con parole sue.
  - Dove nel CSS: sezione di `theme.css` coinvolta.
  - Approccio: nuova opzione Style Settings o modifica di default.
  - Impatto sulla versione: patch (fix) o minor (nuova opzione).

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
8. Rispondere sul thread Reddit con le novità.
