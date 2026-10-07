# Case study: from idea to a live user trial in 12 days

This is how the system took one of my own products, **Bağ**, from an idea to a live 7-day user trial.
Bağ is a daily Turkish word puzzle: 16 words, 4 hidden connections, one puzzle a day.

Dates are real. Numbers come from the agents' own reports.

## Timeline

| Date | Agent | What happened |
|---|---|---|
| 22 Sep | Product | Discovery: target players, competitors, constraints. First PRD (v0.1). |
| 28 Sep | Product, Designer, Puzzle Editor | PRD revised to v0.1.2. Brand identity and design directions. Style guide and a paper playtest of the first puzzles. |
| 28 Sep | Project Manager | Work orders for each agent; QA writes a test plan before any code exists. |
| 30 Sep | Project Manager | **Change request CR-1**: before building a store app, validate the game with a 7-day web trial shared by link. Scope, gates and work orders updated. |
| 30 Sep | Backend Dev | Anonymous event counting API on Cloudflare Pages Functions + D1 (SQLite), 45 tests. |
| 30 Sep | Designer, Copywriter | Design spec for the trial page; approved copy package. |
| 1–2 Oct | Web Dev | Trial site: plain HTML/CSS and ES modules, no framework, no runtime dependencies. |
| 2 Oct | Designer | Design review: "fit with fixes", 3 items (D1–D3). |
| 2 Oct | QA | Release check: **conditionally ready**, 4 medium + 6 low findings. |
| 2 Oct | Web Dev | Fix round. |
| 2 Oct | QA | Retest: **10 / 10 closed**, design items 3 / 3 hold, 2 new low findings (one accepted as known risk, one fixed in a final round). |
| 1–3 Oct | DevOps | Infrastructure set up (1 Oct), release approved, live on 3 Oct. |
| 4–10 Oct | — | Live trial with real players. |

## What the gates caught

The developer agent reported its own work as green: unit tests and browser scripts passed. The independent
QA agent still found ten issues, because it tested behaviour, not just code paths. A few examples:

- **Yesterday's unplayed puzzle opened instead of today's.** The product team then defined the rule:
  a game counts as "in progress" only after at least one submitted guess.
- **Failed analytics events were never retried.** After the fix, QA cut the server, played a full game,
  restored the server and confirmed each of the five events was sent exactly once.
- **A hint message stayed on screen for the next guesses.** Measured at 20, 120 and 420 ms after the
  second submission.
- **Keyboard focus was lost when the welcome dialog closed.** Checked with Esc, Enter and mouse, including
  whether the focus ring shows only for keyboard users.

QA also verified the counting service end to end: for the same 3 devices × 3 days scenario, all 280 values in
the summary matched a hand calculation.

## Final state at release

- 110 / 110 unit tests (`node:test`)
- Browser automation: 58 main-flow checks + 47 scenario checks (Puppeteer)
- Visual definition of done: 96 viewport / state combinations with no overflow, contrast above WCAG AA,
  no touch targets below size, no console errors
- Formatting check clean
- Puzzle data in the build verified byte-for-byte against the editor's approved package

## Takeaways

- Writing the test plan before the code made QA independent from the developer's view of "done".
- A cheap web trial (CR-1) before a store app was the right call: it tests the game, not the packaging.
- Every finding had an owner, a severity and a retest. Nothing was closed because someone said it was fixed.
