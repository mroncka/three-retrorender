# three-retrorender

> Retro pre-rendered sprite sheet baking for Three.js — inspired by the Unity [RetroRender](https://awtdev.itch.io/retrorender) asset.

Bake any `THREE.Object3D` (GLTF, geometry, skinned mesh) into a **directional sprite sheet** with one call. Drop the resulting texture + atlas into any Three.js scene and use the included `RetroSprite` helper for automatic view-angle billboarding.

## Features

- **`SpriteBaker`** — offscreen `WebGLRenderTarget` pipeline; bakes N angles × M frames into a single sprite sheet
- **`RetroSprite`** — `THREE.Sprite`-based billboard that picks the correct atlas frame from camera angle in real time
- **Pixel-art post-process** — integer downscale + optional palette quantisation (ordered dither / floyd-steinberg)
- **Animation support** — bakes `AnimationMixer` timelines frame-by-frame
- **GLTF-ready** — convenience loader wrapping `GLTFLoader` → bake in one line
- **Atlas JSON** — exports frame metadata compatible with PixiJS, Phaser, and custom engines
- **Zero runtime deps** — peer dep on `three` only; optional `sharp` for Node.js CLI baking

## Packages

| Package | Description |
|---|---|
| `packages/core` | Baking engine + `RetroSprite` |
| `packages/cli` | Node.js CLI for offline batch baking |
| `packages/react` | `<RetroSprite>` React Three Fiber component |
| `apps/demo` | Interactive Vite demo |

## Quick Start

```ts
import * as THREE from 'three'
import { SpriteBaker, RetroSprite } from '@three-retrorender/core'

const baker = new SpriteBaker(renderer, {
  spriteSize: 64,      // output pixel size per frame
  angles: 8,           // directional frames (N/NE/E/SE/S/SW/W/NW)
  pixelScale: 4,       // integer downscale factor for crunchy pixels
})

const { texture, atlas } = await baker.bake(myGltfScene)

const sprite = new RetroSprite(texture, atlas)
scene.add(sprite)

// in your render loop:
sprite.update(camera)
```

## Development

```bash
pnpm install
pnpm dev          # starts Vite demo
pnpm build        # builds all packages
pnpm test
```

## Roadmap

See [open issues](https://github.com/mroncka/three-retrorender/issues) for the full milestone plan.

## License

MIT
