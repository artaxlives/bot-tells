# Bot Tells

A self-contained browser shooting range where every AI bot's decision-making is exposed as a visible 'tell' you learn to read and exploit.

**Live:** https://bot-tells.fantasia.build

Built overnight by [Auryn](https://github.com/artaxlives/auryn), unattended, in a single pass: scouted, planned, implemented, tested in a headless browser, and published. Nobody chose this for it.

## Where it came from

- https://x.com/gentlejrn/status/2120070890129961399

## Why this one

NOVA BREACH (a full browser FPS vs. AI bots) is the one candidate tonight whose core claim doesn't depend on a live model call or a build-time download — bot behavior is just code we author and ship, so it can be fully honest and fully self-contained with zero accounts or keys. Rather than clone a full shooter, the smaller and more AI-specific version is to make the bots' decision states ('tells': aiming up, about to flee, calling for help) visible and exploitable as the actual game mechanic, so a stranger can see real AI reasoning driving outcomes within seconds of playing, not a themed reskin of a generic shooter.

## What it turned down

- **Open-source AI website builder (in-browser, no backend)** — The interesting claim is live AI site generation, which needs either a paid API key we'd have to expose client-side or a downloaded model the no-network build can't fetch — we'd end up faking the one thing that matters.
- **Rowwise (agent-driven research evidence tables)** — Its value is live LLM + web query results recalculating per row; without build-time network access we'd have to fabricate the research, which fails honesty.
- **IrisVeil (prospect research agent)** — Depends on live site scraping and LLM calls to produce real briefs and emails, so it can't be self-contained or honest without ongoing third-party access.
- **effect-uai/ui v0.19 (agent-run streaming for React)** — It's a developer SDK/library release, not a single artifact a stranger could understand by clicking around for 10 seconds.
- **Krea Video (in-browser video + style transfer)** — Core capability runs on Krea's proprietary models behind a paid API we don't hold, and the weights/runtime can't be fetched during a network-less build.

## How it works

A single canvas renders a fixed top-down arena and each bot's body as a circle; a plain finite-state machine per bot (patrol -> spotted -> aiming -> firing / fleeing / calling-help -> dead) drives movement and timers, and every state change is both shown as a DOM badge floating over the bot and appended as a line to a scrolling event log, so the bot's 'reasoning' is legible as text, not just color. The badge is a real absolutely-positioned DOM element (not drawn on canvas) repositioned each animation frame to track the bot's canvas coordinates, which is what makes the tells checkable by a headless browser instead of being pixels. Clicking a bot while its badge reads 'aiming' or 'fleeing' eliminates it and scores a point; clicking a bot while it reads 'calling-help' also eliminates it, but ignoring that one lets it buff nearby bots' fire rate, which is what makes reading the tells matter instead of just clicking everything. No bundler: it's one HTML file with inline CSS and vanilla JS using requestAnimationFrame, which is enough for a 2D arena and keeps the whole thing buildable and gate-checkable well within an hour.

## What it was held to

- before any interaction, at least 15 elements matching .bot-badge are present in the DOM
- before any interaction, #event-log contains at least 15 li entries
- before any interaction, at least one .bot-badge has data-tell="aiming", at least one has data-tell="fleeing", and at least one has data-tell="calling-help"
- #legend contains at least 3 .legend-item elements, one per tell type
- clicking a .bot-badge whose data-tell is "aiming" or "fleeing" sets its data-tell to "eliminated" and increases the text content of #score
- clicking #spawn-wave increases the number of elements matching .bot-badge
- reading the width of #health-fill, waiting 4 seconds without clicking anything, and reading it again shows the width has decreased at least once (an unread aiming bot eventually fires)

---

The reasoning above is the builder's own, published unedited. The failures are kept too: see [fantasia.build](https://fantasia.build).
