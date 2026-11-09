# Kinesis Parallel — Architecture

## Skill layer

The `.grok/skills/` directory is a versioned knowledge base. Each skill is a self-contained reference: a `SKILL.md` with doctrine (the why), pipeline steps (the how), and QC checks (the verification). Skills cross-reference each other through explicit load instructions — a sprite skill loads `game-asset-core` plus a specialist, never reinvents the base.

## Core skills

| Skill | Role |
| --- | --- |
| `game-asset-core` | Magenta-key chroma standards, engine-ready defaults, blind verify, retry discipline |
| `game-animation-frames` | Motion and loop laws for sprites and sheets |
| `game-character-consistency` | Turnarounds and variants across angles |
| `game-tilesets` | Seamless tiles and transitions |
| `game-ui-icons` | HUD, buttons, icon sets |
| `building-games` | 3D libs, pathfinding, audio, collision, ECS, game feel |
| `controls` | Input handling, key mapping, control-sign verification |
| `design-ui` | Refined UI, surfaces, typography, animations, performance |
| `face-template` | Facial topology templates for avatar generation |
| `auth` | Better Auth wiring, federation, session UI, per-user data |

## Reference layer

`.grok/references/` holds implementation detail the skills point to:

- `scaffold.md` — first-scaffold snippets matching the installed TanStack Start
- `deploy-target.md` — Vercel/Nitro build contracts, port discipline
- `data-and-auth.md` — database dual-mode (Postgres/PGLite), migrations, server functions, auth wiring
- `generated-art.md` — 2D asset pipelines, share cards, game art doctrine
- `browser-qa.md` — smoke passes, interactive flows, depth judgment
- `hibernate-revive.md` — session lifecycle, startup.sh rules, multi-turn continuity

## Swarm orchestration

The seven-desk reconstruction swarm is defined here as skill contracts: each desk's input artifact type, output artifact type, confidence scoring, and merge behavior. The front atelier consumes these contracts; this repo defines them.

## DCC integration points

Export and round-trip paths for Maya, Blender, Houdini, ZBrush, COLMAP, Metashape, and RealityCapture. glTF is the interchange format for web delivery; native DCC formats for artist round-trips.

## Auth architecture

Better Auth at `/api/auth/*` with Grok broker federation for Google and X. Local email-password opt-in. `authMiddleware` on every server function. Connector isolation: browser never touches connector APIs directly.

## Deploy architecture

Nitro `vercel` preset, build-gated. Dual-port: 8080 live, 8081 QA-only. PWA plugin baked at build time. Asset base path must resolve `/assets/*` in production.