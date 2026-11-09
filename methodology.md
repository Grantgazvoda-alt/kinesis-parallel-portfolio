# Kinesis Parallel — Methodology

## Skill doctrine

Every skill follows the same structure: **doctrine** (why this exists), **pipeline** (ordered steps), **QC** (verification that the output meets the doctrine). A skill without QC is a suggestion; a skill with QC is a contract.

## Game asset core

**Doctrine:** all 2D game assets ship on magenta-key sheets (`#FF00FF`) so a single chroma postprocess strips backgrounds universally. No per-asset key colors.

**Pipeline:** generate sprite → magenta sheet → local chroma script → verify → retry on failure.

**QC:** blind verify — a second pass checks the sheet without knowing what was generated. Retry discipline: two failures on the same step triggers fallback (different tool or manual), never a third blind attempt.

## Animation frames

**Doctrine:** motion must obey loop laws — a walk cycle's first and last frames must be identical in pose, or the loop pops.

**Pipeline:** define the motion arc → generate frames → check loop closure → adjust timing.

**QC:** frame-to-frame delta analysis catches pops and foot-skate before shipping.

## Character consistency

**Doctrine:** a character seen from four angles must read as the same character. Proportions, palette, and silhouette are invariants.

**Pipeline:** generate turnaround → compare against invariants → regenerate divergences.

**QC:** automated proportion checks plus a consistency pass that flags palette drift.

## Tilesets

**Doctrine:** tiles must seam seamlessly in all four directions. No visible edges at tile boundaries.

**Pipeline:** generate tile → seam-check all four edges → fix transitions.

**QC:** rendered composite test — a 3×3 grid of the tile must show no seams.

## UI icons

**Doctrine:** icon sets share a visual language — stroke weight, corner radius, optical size.

**Pipeline:** define the language → generate the set → consistency check.

**QC:** side-by-side review against the defined language.

## Building games (3D)

**Doctrine:** 3D games need an ECS architecture, collision that matches the visual geometry, and game feel that sells the motion.

**Pipeline:** ECS setup → collision shapes from mesh bounds → pathfinding layer → audio → juice (screenshake, particles, feedback).

**QC:** control-sign verification (A left / D right from chase cam), collision stress test, feel pass.

## Controls

**Doctrine:** input must be predictable. Inverted axes are bugs, not preferences.

**Pipeline:** map keys to actions → verify signs → test edge cases (simultaneous keys, held keys).

**QC:** automated sign checks per control domain.

## Design UI

**Doctrine:** refined UI means consistent surfaces, typography, and motion. Performance is part of design — a beautiful interface that janks is not refined.

**Pipeline:** surface system → type scale → animation tokens → performance budget.

**QC:** performance profiling against the budget.

## Face template

**Doctrine:** avatar faces share a base topology so deformation layers (cartoon, lifelike) plug in without retopology.

**Pipeline:** base mesh → landmark anchors → muscle attachment points → export template.

**QC:** deformation test on the template before any performer-specific work.

## Auth

**Doctrine:** identity is server-resolved. The browser holds a session cookie, not a trusted ID.

**Pipeline:** flag check → schema → routes → middleware → UI gates.

**QC:** `check:auth` run against a live dev server — exit 0 means the server and next build agree on the auth flag.

## Data

**Doctrine:** migrations are the single schema source. Ordered SQL files, not inline definitions.

**Pipeline:** add table as ordered migration → apply to Neon on deploy and PGLite on preview startup.

**QC:** migration dry-run before shipping.

## Deploy

**Doctrine:** what works in dev must work in prod. Dev-only code that breaks the build is a bug.

**Pipeline:** edit → `npm run build` → `preview:restart` → smoke built output → ship.

**QC:** built-output smoke pass with baseline comparison — only re-read screenshots when the verdict flags divergence.

## Browser QA

**Doctrine:** the agent is the QA, not the user. Every check is a verdict with an exit code.

**Pipeline:** mandatory smoke (desktop + mobile screenshots + JSON) → interactive batches with throwing evals → depth by judgment.

**QC:** falsy is not failure. Throwing is. A 404 ticks a box too — assert on content.

## Hibernate and revive

**Doctrine:** a session that comes back must come back working. `startup.sh` is the contract.

**Pipeline:** write `startup.sh` that stops stale previews, checks 8080, starts dev if needed.

**QC:** revive test — kill the session, restore, verify 8080 answers.