# Embedding Patterns

## Plain HTML (No Framework)

Add the script tag once, then use the web component anywhere.

```html
<!-- In <head> or before </body> -->
<script type="module" src="https://unpkg.com/@lottiefiles/dotlottie-wc@latest/dist/dotlottie-wc.js"></script>

<!-- Place where animation should appear -->
<dotlottie-wc
  src="https://lottie.host/{animation-url}.lottie"
  autoplay
  loop
  style="width: 200px; height: 200px;"
></dotlottie-wc>
```

No npm install required. The web component registers itself globally.

## React / Next.js

```bash
npm install @lottiefiles/dotlottie-react
```

```tsx
import { DotLottieReact } from '@lottiefiles/dotlottie-react';

function LoadingAnimation() {
  return (
    <DotLottieReact
      src="https://lottie.host/{animation-url}.lottie"
      autoplay
      loop
      style={{ width: 200, height: 200 }}
    />
  );
}
```

### With playback control (ref callback)

```tsx
import { DotLottieReact } from '@lottiefiles/dotlottie-react';
import { useState } from 'react';
import type { DotLottie } from '@lottiefiles/dotlottie-web';

function ControlledAnimation() {
  const [dotLottie, setDotLottie] = useState<DotLottie | null>(null);

  return (
    <>
      <DotLottieReact
        src="https://lottie.host/{animation-url}.lottie"
        dotLottieRefCallback={setDotLottie}
        autoplay
        loop
      />
      <button onClick={() => dotLottie?.play()}>Play</button>
      <button onClick={() => dotLottie?.pause()}>Pause</button>
      <button onClick={() => dotLottie?.stop()}>Stop</button>
    </>
  );
}
```

## Vue / Nuxt

```bash
npm install @lottiefiles/dotlottie-vue
```

```vue
<script setup>
import { DotLottieVue } from '@lottiefiles/dotlottie-vue';
</script>

<template>
  <DotLottieVue
    src="https://lottie.host/{animation-url}.lottie"
    autoplay
    loop
    style="width: 200px; height: 200px;"
  />
</template>
```

### With playback control

```vue
<script setup>
import { ref } from 'vue';
import { DotLottieVue } from '@lottiefiles/dotlottie-vue';

const dotLottie = ref(null);

function onDotLottieRef(instance) {
  dotLottie.value = instance;
}
</script>

<template>
  <DotLottieVue
    src="https://lottie.host/{animation-url}.lottie"
    autoplay
    loop
    @dotlottie-ref="onDotLottieRef"
  />
  <button @click="dotLottie?.play()">Play</button>
  <button @click="dotLottie?.pause()">Pause</button>
</template>
```

## Svelte / SvelteKit

```bash
npm install @lottiefiles/dotlottie-svelte
```

```svelte
<script>
  import { DotLottieSvelte } from '@lottiefiles/dotlottie-svelte';
</script>

<DotLottieSvelte
  src="https://lottie.host/{animation-url}.lottie"
  autoplay
  loop
  style="width: 200px; height: 200px;"
/>
```

### With playback control

```svelte
<script>
  import { DotLottieSvelte } from '@lottiefiles/dotlottie-svelte';

  let dotLottie;

  function handleRef(instance) {
    dotLottie = instance;
  }
</script>

<DotLottieSvelte
  src="https://lottie.host/{animation-url}.lottie"
  autoplay
  loop
  dotLottieRefCallback={handleRef}
/>
<button on:click={() => dotLottie?.play()}>Play</button>
<button on:click={() => dotLottie?.pause()}>Pause</button>
```

## Common Props (All Frameworks)

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `src` | string | required | URL to `.lottie` or `.json` file |
| `autoplay` | boolean | false | Start playing immediately |
| `loop` | boolean | false | Loop the animation |
| `speed` | number | 1 | Playback speed (0.5 = half, 2 = double) |
| `backgroundColor` | string | transparent | Canvas background color |
| `direction` | 1 \| -1 | 1 | Forward (1) or reverse (-1) |
| `mode` | string | "forward" | "forward", "reverse", "bounce", "reverse-bounce" |
| `segment` | [number, number] | - | Play only frames [start, end] |

## Series Barrel File Pattern

When embedding multiple related animations, create a central config:

```typescript
// src/animations/index.ts
export const animations = {
  loading: "https://lottie.host/abc123/loader.lottie",
  success: "https://lottie.host/def456/check.lottie",
  error: "https://lottie.host/ghi789/error.lottie",
  empty: "https://lottie.host/jkl012/empty.lottie",
  notFound: "https://lottie.host/mno345/404.lottie",
} as const;

export type AnimationKey = keyof typeof animations;
```

### React series wrapper

```tsx
import { DotLottieReact } from '@lottiefiles/dotlottie-react';
import { animations, type AnimationKey } from '@/animations';

interface LottieAnimationProps {
  name: AnimationKey;
  size?: number;
  loop?: boolean;
  speed?: number;
}

export function LottieAnimation({ name, size = 200, loop = true, speed = 1 }: LottieAnimationProps) {
  return (
    <DotLottieReact
      src={animations[name]}
      autoplay
      loop={loop}
      speed={speed}
      style={{ width: size, height: size }}
    />
  );
}
```

Usage: `<LottieAnimation name="loading" size={64} />`

### Vue series wrapper

```vue
<!-- src/components/LottieAnimation.vue -->
<script setup>
import { DotLottieVue } from '@lottiefiles/dotlottie-vue';
import { animations } from '@/animations';

const props = defineProps({
  name: { type: String, required: true },
  size: { type: Number, default: 200 },
  loop: { type: Boolean, default: true },
  speed: { type: Number, default: 1 },
});
</script>

<template>
  <DotLottieVue
    :src="animations[name]"
    autoplay
    :loop="loop"
    :speed="speed"
    :style="{ width: size + 'px', height: size + 'px' }"
  />
</template>
```

### Svelte series wrapper

```svelte
<!-- src/lib/components/LottieAnimation.svelte -->
<script>
  import { DotLottieSvelte } from '@lottiefiles/dotlottie-svelte';
  import { animations } from '$lib/animations';

  export let name;
  export let size = 200;
  export let loop = true;
  export let speed = 1;
</script>

<DotLottieSvelte
  src={animations[name]}
  autoplay
  {loop}
  {speed}
  style="width: {size}px; height: {size}px;"
/>
```

## Speed Guidelines by Brand Energy

| Brand Energy | Speed Value | Effect |
|-------------|-------------|--------|
| Quiet / Minimal | 0.5 - 0.75 | Slow, meditative motion |
| Balanced / Professional | 0.8 - 1.0 | Natural pace |
| Energetic / Playful | 1.0 - 1.5 | Upbeat, responsive |
| Urgent / Dynamic | 1.5 - 2.0 | Fast, attention-grabbing |
