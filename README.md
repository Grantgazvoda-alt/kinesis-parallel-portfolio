# Kinesis Parallel Track — Skill and Reference Layer

Portfolio for the parallel development track of Kinesis, Official Intelligence's live 3D animation system. This repo holds the skill and reference layer the front atelier draws from. Architecture and methodology are public; implementation stays proprietary.

## What it covers

- **Photogrammetry pipelines**: FAST detection, sparse/dense reconstruction, bundle adjustment, mesh and texture stages
- **Photo-avatar**: single-image head mesh generation with facial landmark priors
- **Lip sync**: three algorithms — viseme-driven, audio-feature-driven, hybrid
- **Écorché deformation**: cartoon and lifelike muscle layers
- **DCC integration**: Maya, Blender, Houdini, ZBrush, COLMAP, Metashape, RealityCapture
- **Browser QA**: Playwright smoke passes and agent-browser interactive flows
- **Session lifecycle**: hibernate, revive, and multi-turn continuity
- **Auth and data**: Better Auth federation, server-function gating, connector isolation

## Stack

TanStack Start · React 19 · Three.js / React Three Fiber · Zustand · Tailwind v4 · Vercel/Nitro deploy

## Public materials

- `architecture.md` — skill-layer structure, swarm orchestration, DCC integration points
- `methodology.md` — doctrine, pipeline steps, and QC checks per skill domain
- `dcc-integration.md` — how Kinesis connects to external DCC tools
- `browser-qa.md` — agent-driven QA methodology
- `PROPRIETARY-LICENSE.md` — implementation remains closed

## Signal for reviewers

Each skill is a self-contained reference with doctrine, pipeline steps, and QC checks — not a pile of prompts. The game-asset-core skill defines magenta-key chroma standards that downstream sprite and tileset skills depend on. Animation-frames defines motion and loop laws. Character-consistency defines turnaround rules. The skill graph is versioned and cross-referenced, which is what makes a multi-agent swarm reliable instead of chaotic.