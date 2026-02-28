# Rive Embedding Patterns

## Plain HTML (No Framework)

Use the canvas runtime via unpkg CDN. Rive requires a `<canvas>` element and JS instantiation (no web component like Lottie).

```html
<!-- In <head> or before </body> -->
<script src="https://unpkg.com/@rive-app/canvas@2.24.0"></script>

<!-- Canvas where animation renders -->
<canvas id="rive-canvas" width="400" height="400" style="width: 200px; height: 200px;"></canvas>

<script>
  const r = new rive.Rive({
    src: '/animations/my-animation.riv',
    canvas: document.getElementById('rive-canvas'),
    autoplay: true,
    stateMachines: 'State Machine 1',
    onLoad: () => {
      r.resizeDrawingSurfaceToCanvas();
    },
  });
</script>
```

**Important:** Always call `resizeDrawingSurfaceToCanvas()` in `onLoad` for crisp rendering on Retina/high-DPI displays.

No npm install required when using the CDN. The `rive` global is available after the script loads.

### Self-hosting .riv files

Rive has no public CDN like lottie.host. Always self-host `.riv` files:

```bash
# Download from Rive Marketplace or export from editor
# Place in your public directory
mkdir -p public/animations
cp my-animation.riv public/animations/
```

Then reference as `/animations/my-animation.riv` in the `src` property.

### Rive constructor options

```javascript
new rive.Rive({
  src: '/animations/my-animation.riv',
  canvas: document.getElementById('canvas'),
  autoplay: true,
  stateMachines: 'State Machine 1',
  onLoad: () => { /* resize, access inputs */ },
  onStateChange: (event) => { /* state changed */ },
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `src` | string | required | URL or path to `.riv` file |
| `canvas` | HTMLCanvasElement | required | Target canvas element |
| `autoplay` | boolean | false | Start playing immediately |
| `stateMachines` | string \| string[] | - | State machine name(s) to load |
| `animations` | string \| string[] | - | Animation name(s) to play (if not using state machines) |
| `artboard` | string | first | Artboard name (if `.riv` has multiple) |
| `onLoad` | function | - | Callback when animation loads |
| `onStateChange` | function | - | Callback on state machine transitions |

## React

```bash
npm install @rive-app/react-webgl2
```

### Simple component

```tsx
import Rive from '@rive-app/react-webgl2';

function MyAnimation() {
  return (
    <Rive
      src="/animations/my-animation.riv"
      stateMachines="State Machine 1"
      style={{ width: 200, height: 200 }}
    />
  );
}
```

### With state machine control

```tsx
import { useRive, useStateMachineInput } from '@rive-app/react-webgl2';

function InteractiveButton() {
  const { rive, RiveComponent } = useRive({
    src: '/animations/button.riv',
    stateMachines: 'Button State',
    autoplay: true,
  });

  const hoverInput = useStateMachineInput(rive, 'Button State', 'isHovered');
  const clickTrigger = useStateMachineInput(rive, 'Button State', 'onClick');

  return (
    <RiveComponent
      style={{ width: 200, height: 60 }}
      onMouseEnter={() => hoverInput && (hoverInput.value = true)}
      onMouseLeave={() => hoverInput && (hoverInput.value = false)}
      onClick={() => clickTrigger?.fire()}
    />
  );
}
```

### Rive component props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `src` | string | required | URL or path to `.riv` file |
| `stateMachines` | string \| string[] | - | State machine name(s) |
| `animations` | string \| string[] | - | Animation name(s) |
| `artboard` | string | first | Artboard name |
| `autoplay` | boolean | true | Start playing immediately |
| `style` | CSSProperties | - | Container styles |

### useRive hook

```tsx
const { rive, RiveComponent } = useRive({
  src: '/animations/my-animation.riv',
  stateMachines: 'State Machine 1',
  autoplay: true,
});
```

Returns:
- `rive` — the Rive instance (null until loaded). Use for `.play()`, `.pause()`, `.stop()`.
- `RiveComponent` — React component rendering the canvas. Must be in the DOM for Rive to instantiate.

### useStateMachineInput hook

```tsx
const input = useStateMachineInput(rive, 'stateMachineName', 'inputName');
```

Returns the input object or `null` (until rive loads). Input types:
- **Trigger**: call `input.fire()`
- **Boolean**: set `input.value = true/false`
- **Number**: set `input.value = 0.5`

## Vue

No dedicated Vue package. Use the canvas runtime directly.

```bash
npm install @rive-app/canvas
```

```vue
<template>
  <canvas ref="canvasRef" :style="{ width: size + 'px', height: size + 'px' }"></canvas>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { Rive } from '@rive-app/canvas';

const props = defineProps({
  src: { type: String, required: true },
  stateMachine: { type: String, default: 'State Machine 1' },
  size: { type: Number, default: 200 },
});

const canvasRef = ref(null);
let riveInstance = null;

onMounted(() => {
  riveInstance = new Rive({
    src: props.src,
    canvas: canvasRef.value,
    autoplay: true,
    stateMachines: props.stateMachine,
    onLoad: () => {
      riveInstance.resizeDrawingSurfaceToCanvas();
    },
  });
});

onUnmounted(() => {
  riveInstance?.cleanup();
});
</script>
```

### With state machine control (Vue)

```vue
<template>
  <canvas ref="canvasRef" style="width: 200px; height: 200px;"></canvas>
  <button @click="fireAction">Trigger</button>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { Rive } from '@rive-app/canvas';

const canvasRef = ref(null);
let riveInstance = null;
let actionTrigger = null;

onMounted(() => {
  riveInstance = new Rive({
    src: '/animations/component.riv',
    canvas: canvasRef.value,
    autoplay: true,
    stateMachines: 'Main',
    onLoad: () => {
      riveInstance.resizeDrawingSurfaceToCanvas();
      const inputs = riveInstance.stateMachineInputs('Main');
      actionTrigger = inputs.find(i => i.name === 'action');
    },
  });
});

function fireAction() {
  actionTrigger?.fire();
}

onUnmounted(() => {
  riveInstance?.cleanup();
});
</script>
```

## Svelte / SvelteKit

No dedicated Svelte package. Use the canvas runtime with Svelte actions.

```bash
npm install @rive-app/canvas
```

```svelte
<script>
  import { Rive } from '@rive-app/canvas';
  import { onDestroy } from 'svelte';

  export let src;
  export let stateMachine = 'State Machine 1';
  export let size = 200;

  let riveInstance;

  function riveAction(canvas) {
    riveInstance = new Rive({
      src,
      canvas,
      autoplay: true,
      stateMachines: stateMachine,
      onLoad: () => {
        riveInstance.resizeDrawingSurfaceToCanvas();
      },
    });

    return {
      destroy: () => riveInstance?.cleanup(),
    };
  }

  onDestroy(() => riveInstance?.cleanup());
</script>

<canvas use:riveAction style="width: {size}px; height: {size}px;"></canvas>
```

### With state machine control (Svelte)

```svelte
<script>
  import { Rive } from '@rive-app/canvas';

  export let src;

  let riveInstance;
  let trigger;

  function riveAction(canvas) {
    riveInstance = new Rive({
      src,
      canvas,
      autoplay: true,
      stateMachines: 'Main',
      onLoad: () => {
        riveInstance.resizeDrawingSurfaceToCanvas();
        const inputs = riveInstance.stateMachineInputs('Main');
        trigger = inputs.find(i => i.name === 'action');
      },
    });

    return { destroy: () => riveInstance?.cleanup() };
  }
</script>

<canvas use:riveAction style="width: 200px; height: 200px;"></canvas>
<button on:click={() => trigger?.fire()}>Fire</button>
```

## State Machine Input Types

All frameworks use the same underlying API to control state machines:

| Input Type | How to Set | Example |
|------------|-----------|---------|
| **Trigger** | `.fire()` | Button click, submit action |
| **Boolean** | `.value = true/false` | Hover state, toggle, enabled/disabled |
| **Number** | `.value = 0.5` | Progress bar, slider, scroll position |

Access inputs after the Rive instance loads:

```javascript
const inputs = riveInstance.stateMachineInputs('State Machine Name');
const myTrigger = inputs.find(i => i.name === 'triggerName');
const myBool = inputs.find(i => i.name === 'isHovered');
const myNumber = inputs.find(i => i.name === 'progress');

myTrigger.fire();
myBool.value = true;
myNumber.value = 0.75;
```

## Series Barrel File Pattern

Same pattern as Lottie series, but with `.riv` paths (self-hosted):

```typescript
// src/animations/index.ts
export const animations = {
  toggle: '/animations/toggle.riv',
  button: '/animations/button.riv',
  checkbox: '/animations/checkbox.riv',
  loading: '/animations/loading.riv',
  success: '/animations/success.riv',
} as const;

export type AnimationKey = keyof typeof animations;
```

### React series wrapper

```tsx
import { useRive } from '@rive-app/react-webgl2';
import { animations, type AnimationKey } from '@/animations';

interface RiveAnimationProps {
  name: AnimationKey;
  size?: number;
  stateMachine?: string;
}

export function RiveAnimation({ name, size = 200, stateMachine = 'State Machine 1' }: RiveAnimationProps) {
  const { RiveComponent } = useRive({
    src: animations[name],
    stateMachines: stateMachine,
    autoplay: true,
  });

  return <RiveComponent style={{ width: size, height: size }} />;
}
```

Usage: `<RiveAnimation name="toggle" size={48} />`

## Speed Guidelines by Brand Energy

Rive doesn't have a simple `speed` prop like Lottie. To control pacing:

1. **Design-time**: Set animation duration in the Rive editor
2. **Runtime number inputs**: If the Rive file exposes a speed/rate number input, set it via state machine
3. **Playback rate**: Use `rive.play()` with custom time advance in the low-level API (advanced)

For most cases, animation pacing should be baked into the `.riv` file at design time.
