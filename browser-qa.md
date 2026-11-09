# Kinesis Parallel — Browser QA Methodology

Agent-driven quality assurance. The user is not the QA.

## Mandatory smoke pass

One run loads desktop (1280×800) and mobile (390×844). Screenshots both viewports. Prints a JSON verdict: per-viewport console errors, body text, horizontal overflow.

```bash
mkdir -p /workspace/screenshots
node scripts/browser-smoke.mjs http://127.0.0.1:8080/ /workspace/screenshots/app.png
```

Output: desktop PNG, mobile PNG (suffixed `-mobile`), and JSON verdict. Read both PNGs in one batched pass if an image tool is available; iterate if either looks wrong.

## Built-output check

Serve with `npm run preview:restart` (loopback 127.0.0.1:8081 — QA only, never live). Reuse the dev verdict as baseline:

```bash
npm run preview:restart
node scripts/browser-smoke.mjs http://127.0.0.1:8081/ /workspace/screenshots/built.png --baseline /workspace/screenshots/dev.json
npm run preview:stop
```

The JSON reports `divergesFromBaseline` — only re-read screenshots when it flags.

## Interactive passes

`agent-browser` drives the page: open, snapshot, click, fill, press, wait, eval, screenshot, close.

**Any multi-verb flow goes in one `batch`.** Each invocation is its own tool call; a 10-verb flow costs ten round trips. `batch` runs it in one.

```bash
agent-browser batch --bail <<'JSON'
[["open","http://127.0.0.1:8080/"],
 ["find","text","Start","click"],
 ["fill","#title","Test"],
 ["press","Enter"],
 ["wait","300"],
 ["eval","if (!condition) throw Error('failed')"],
 ["screenshot","/workspace/screenshots/step.png"]]
JSON
```

That is the shape for any app: drive what a user would do, assert with a throwing eval. Exits 0 when it lands, 1 when it doesn't. One tool call is the whole verdict.

## Rules

- **`open` once.** The page outlives the command. A second `open` re-navigates and discards state.
- **A tick is not "loaded".** A 404 reports ✓ too. Assert on content — a `find`, a throwing `eval`.
- **`@ref`s go stale.** Re-snapshot after any DOM change.
- **`find text` is case-sensitive.** Copy the label from `snapshot -i`; guessing costs a failed command.
- **Keys:** `press <key>` sends `event.code`. Raw keystrokes are `keyboard type`. `keydown`/`keydown` send empty code — don't build checks on them.
- **Falsy is not failure.** `eval "false"` exits 0. Throwing is what makes it a verdict.
- **Anything past one expression goes in an IIFE** so it can be re-run.
- **`--bail` stops at the first failure**; without it, later steps still run.
- **Fallback:** if `agent-browser` fails twice on the same step, fall back to a short Playwright script and say so in one line.

## Depth judgment

Landing page: smoke pass is enough. Game with WASD: verify control signs. App with forms: interactive batches on the core verbs. Match depth to what the app does.