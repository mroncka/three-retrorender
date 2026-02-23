# Agent Instructions — three-retrorender

This file is read by AI relay nodes on startup. Follow these rules when working on this repo.

## Stack
- pnpm monorepo + Turborepo
- TypeScript throughout (strict mode)
- Three.js peer dependency — never bundle Three.js into the output
- Vitest for unit tests, Playwright for visual regression
- Vite for the demo app

## Packages

### `packages/core`
- Pure TS, no DOM deps in core baking logic (use `OffscreenCanvas` where possible)
- `SpriteBaker` owns the render pipeline; keep it renderer-agnostic (accept `THREE.WebGLRenderer`)
- `RetroSprite` must work in any Three.js `Scene` without R3F
- Exports: named only, no default exports

### `packages/cli`
- Node.js, uses `node-three-gl` or headless `puppeteer` to get a WebGL context
- Input: GLTF file path + config JSON; Output: PNG sprite sheet + atlas JSON
- Keep CLI flags minimal; config file drives advanced options

### `packages/react`
- R3F `<RetroSprite>` wraps core `RetroSprite`
- Props mirror `SpriteBaker` config where relevant
- Must not import from `packages/cli`

## Conventions
- All public API types exported from `packages/core/src/types.ts`
- Atlas JSON schema defined in `packages/core/src/atlas.schema.json`
- Sprite sheet PNG naming: `{modelName}_{angles}x{frames}_{size}px.png`
- Branch naming: `feat/`, `fix/`, `chore/`
- Commits: Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`)

## Do Not
- Do not commit built `dist/` folders
- Do not add runtime dependencies beyond `three` peer dep in `packages/core`
- Do not auto-merge PRs; always request review
