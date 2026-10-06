# Synthwave Pro

[English](README.md) • [Italiano](README_it.md)

Tema Obsidian con vibe **VaporWave / RetroWave**, pensato per chi passa molte ore davanti allo schermo: programmatori, ingegneri, lettori intensivi.

![Synthwave Pro — palette Synthwave](background.png)

![Synthwave Pro — palette Tokyo Night](background2.png)

## Filosofia

- **Mai nero puro / mai bianco puro** — sfondo indaco profondo, testo bianco-caldo.
- **Neon usato con parsimonia** — accenti riservati a heading, link, focus, syntax highlighting. Il body resta calmo.
- **Contrasto WCAG AA** sul testo principale.
- **Effetti retro opzionali** (glow, griglia prospettica, scanline CRT): tutti disattivati di default, attivabili da Style Settings.
- **Vibe da programmazione**: heading monospace di default, prefissi `#`, `##` davanti ai titoli, code block con bordo neon, status bar in monospaced. Lo stile di testo **Terminal** mette ogni riga delle note su una griglia monospace fissa, come in una finestra di terminale.

## Installazione

1. Settings → Appearance → Themes → **Manage**, cerca **Synthwave Pro**, poi **Install and use**
2. Installa il plugin [Style Settings](https://github.com/mgmeyers/obsidian-style-settings)
3. Apri Settings → Style Settings → Synthwave Pro

Installazione manuale: copia `manifest.json` e `theme.css` in `<vault>/.obsidian/themes/Synthwave Pro/`, poi seleziona il tema in Settings → Appearance.

Richiede **Obsidian 1.13 o successivo** (dalla 1.13 i colori dei callout hanno un formato diverso). Il tema usa anche `color-mix()`, che richiede un installer di Obsidian recente (Chromium 111 o successivo). Se i colori sembrano sbagliati, scarica l'ultimo installer da obsidian.md.

## Varianti (Style Settings)

| Sezione | Opzioni |
|---|---|
| **Palette** | Synthwave (default) · Outrun · Miami · Tokyo Night · Soft Reading · Catppuccin Frappé, Macchiato, Mocha (Latte in light mode) · profondità dello sfondo (Deep · Medium · Soft) |
| **Typography** | Stile del testo: Classic (heading monospace) · Clean · Terminal (tutto monospace su una griglia di righe fissa) · font del testo e monospace · interlinea · peso del grassetto · larghezza di lettura |
| **Headings** | Large o Compact (dimensione del testo, grassetto, colorati, con banda di sfondo) · heading più grandi in Terminal (sempre sulla griglia di righe) · `#` prima degli heading · dimensione degli heading · scala tra i livelli · interlinea · spazio sopra · peso · colore e intensità della banda |
| **Images** | Attenuazione delle immagini in dark mode · luminosità |
| **Effects** | Neon glow · retro grid · CRT scanlines (tutti off di default) |
| **Callouts** | Stile (Thin border · Flat · Left bar · Neon · Dashed monospace) · colori dalla palette · titolo · icone nascoste · raggio · intensità dello sfondo |
| **Advanced** | Sostituire la dimensione del font di Obsidian · colori personalizzati sopra la palette (accenti, sfondi, testo, heading, link, tag, evidenziazione) |

### Suggerimenti d'uso

- **Sessioni di codice lunghe** → palette `Tokyo Night` o `Soft Reading`, glow off, depth medium.
- **Vibe massima retrowave** → `Synthwave` + grid on + glow on (occhio al mal di testa dopo un'ora).
- **Lettura prolungata di note** → `Soft Reading`, line-height 1.7, stile del testo Clean.

### Aggiornare dalla 1.0.x

Il vecchio interruttore "Monospace headings" ora è **Typography → Text style**. Classic (il default) mantiene gli heading monospace. Se avevi spento gli heading monospace, scegli **Clean**.

## Struttura

```
obsidian-synthwave-pro/
├── manifest.json
├── theme.css
├── background.png      (screenshot, palette Synthwave)
├── background2.png     (screenshot, palette Tokyo Night)
├── LICENSE
├── README.md
└── README_it.md
```

## Anteprima Callout & checkbox

> I blocchi seguenti vengono renderizzati con lo stile completo **solo in Obsidian**. GitHub renderizza un sottoinsieme dei callout (NOTE / TIP / IMPORTANT / WARNING / CAUTION) e mostra i marker custom delle task come checkbox grezzi.

### Callout

> [!NOTE] Nota Standard
> Blocco predefinito. Utile per informazioni generali che non richiedono attenzione particolare.
> Syntax: `> [!NOTE]`

> [!TIP] Consiglio Utile
> Usa i callout per organizzare visivamente le tue note. Puoi annidarne uno dentro l'altro.
> Syntax: `> [!TIP]`

> [!IMPORTANT] Importante
> Queste informazioni sono cruciali per comprendere il contesto successivo.
> Syntax: `> [!IMPORTANT]`

> [!WARNING] Attenzione
> Fai attenzione a questo dettaglio. Potrebbe causare confusione se ignorato.
> Syntax: `> [!WARNING]`

> [!CAUTION] Cautela
> Procedi con estrema cautela. Un'azione errata qui potrebbe portare a perdita di dati o errori gravi.
> Syntax: `> [!CAUTION]`

> [!ABSTRACT] Riepilogo
> Ideale per i sommari. `[!SUMMARY]` è un alias.
> Syntax: `> [!ABSTRACT]` o `> [!SUMMARY]`

> [!INFO] Informazioni
> Dettagli tecnici o dati aggiuntivi.
> Syntax: `> [!INFO]`

> [!QUOTE] Citazione
> "Il miglior modo per predire il futuro è crearlo."
> Syntax: `> [!QUOTE]`

> [!SUCCESS] Operazione Riuscita
> Un processo è stato completato con successo o un'ipotesi verificata.
> Syntax: `> [!SUCCESS]`

> [!QUESTION] Domanda
> Domanda aperta evidenziata come tale.
> Syntax: `> [!QUESTION]`

> [!FAILURE] Errore
> Qualcosa non ha funzionato come previsto o un test è fallito.
> Syntax: `> [!FAILURE]`

> [!DANGER] Pericolo
> Rischio elevato. Usa questo blocco solo per avvertimenti critici.
> Syntax: `> [!DANGER]`

> [!BUG] Bug Segnalato
> Per tracciare errori noti nel codice o software.
> Syntax: `> [!BUG]`

> [!EXAMPLE] Esempio Pratico
> 1. Inizia con `> [!TIPO]`
> 2. Aggiungi il testo nelle righe successive.
> Syntax: `> [!EXAMPLE]`

#### Callout Collassabile

Aggiungi un `-` dopo il tipo per rendere il blocco collassato di default.

> [!TIP]- Clicca per espandere
> Contenuto nascosto finché non clicchi sul titolo.
> Syntax: `> [!TIP]-`

### Custom Checkbox

#### Stati Base
- [ ] Task da fare (default) `[ ]`
- [x] Task completato `[x]`
- [X] Task completato (maiuscolo) `[X]`
- [-] Task annullato / N/A `[-]`

#### Priorità e Urgenza
- [!] **URGENTE** — azione immediata `[!]`
- [>] Task delegato `[>]`
- [<] Task rimandato / schedulato `[<]`
- [/] In corso di svolgimento `[/]`

#### Idee e Creatività
- [*] Idea brillante / preferito `[*]`
- [l] Insight / lampadina `[l]`
- [B] Brainstorming `[B]`
- [Y] Riflessione / pensiero `[Y]`

#### Dubbi e Verifiche
- [?] Dubbio da chiarire `[?]`
- [w] Da revisionare `[w]`
- [V] Verificato `[V]`

#### Sviluppo e Tecnica
- [b] Bug da risolvere `[b]`
- [f] Feature richiesta / bandiera `[f]`
- [k] Chiave / password / critico `[k]`
- [S] Sicuro / bloccato `[S]`
- [c] Cloud / sync `[c]`

#### Organizzazione e Tempo
- [t] Target / obiettivo `[t]`
- [D] Scadenza / data `[D]`
- [O] Tempo stimato `[O]`
- [z] In pausa / snooze `[z]`
- [R] Da aggiornare / refresh `[R]`

#### Comunicazione e Info
- [i] Informazione generale `[i]`
- [n] Nota numerica `[n]`
- [Q] Discussione aperta `[Q]`
- [u] Utente coinvolto `[u]`
- [m] Luogo / mappa `[m]`

#### Vari
- [p] Pin / fisso `[p]`
- [d] Da eliminare / cestino `[d]`
- [C] Da riciclare / rivedere `[C]`
- [E] Link esterno / web `[E]`
- [F] Hot / trending `[F]`
- [H] Home / principale `[H]`
- [J] Celebrazione `[J]`
- [K] Strumento `[K]`
- [L] Lettura / studio `[L]`
- [M] Finanza / costo `[M]`
- [N] Nota rapida `[N]`
- [P] Puntina (variante) `[P]`
- [T] Ticket / supporto `[T]`
- [U] Priorità alta (up) `[U]`
- [W] Lavoro manuale `[W]`
- [a] Audio / registrato `[a]`
- [e] Email da inviare `[e]`
- [s] Salvato / bookmark `[s]`

## Autore

**Ing. Roberto Bissanti** — ingegneria aerospaziale applicata all'innovazione nelle energie rinnovabili (turbine eoliche ad asse verticale, mini-eolico, leghe a memoria di forma), con base a Palermo. Anche sviluppatore software, per i casi in cui il foglio di calcolo non basta più.

Utente Mac dal 1989 — abbastanza vecchio da ricordare quando "modalità scura" significava che lo schermo era spento. Gli occhi non hanno più vent'anni, da qui Synthwave Pro.

- 📬 [roberto.bissanti@gmail.com](mailto:roberto.bissanti@gmail.com)
- 💼 [LinkedIn](https://www.linkedin.com/in/roberto-bissanti/)
- 🐙 [GitHub @robertobissanti](https://github.com/robertobissanti)

Issue, idee e PR sono benvenute su [github.com/robertobissanti/obsidian-synthwave-pro](https://github.com/robertobissanti/obsidian-synthwave-pro).

## Licenza

[MIT](LICENSE) — usa, fork, modifica.

### Crediti

- Ispirazione visiva: [Obsidian SynthWave](https://github.com/marcoluzi/obsidian-synthwave) di [Marco Luzi](https://github.com/marcoluzi).
- Le palette Catppuccin usano i colori ufficiali di [Catppuccin](https://github.com/catppuccin/catppuccin) (licenza MIT, © 2021 Catppuccin).
- Stile di testo Terminal: tecnica della griglia di righe di [Stéphan Zych](https://stephan.zych.be/blog/terminal-style-site-lit-eleventy/).

### Asset di terze parti

- **[Lucide](https://lucide.dev)** icons (licenza ISC) — usate per i glifi dei checkbox custom.
