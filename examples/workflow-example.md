# Workflow Example

## Example 1: Single Animation — Loading Spinner for a Minimal Brand

**User invokes:** `/lottie-design loading spinner`

### Step 1: Detect Project Context

```
Glob: package.json → found
Read: package.json → "next": "14.x", "react": "18.x" in dependencies
       No @lottiefiles packages installed yet.

Glob: DESIGN.md → found
Framework: Next.js (React)
Brand context: DESIGN.md present
```

Output: "Detected Next.js (React) project. Found DESIGN.md with brand context. No Lottie packages installed yet."

### Step 2: Understand Brand Attributes

```
Read: DESIGN.md → extract:
  Emotive Narrative: "Quiet Infrastructure" — minimal, warm, functional
  Visual Philosophy: geometric, clean lines, muted earth tones
  Colors: dark background (#1A1916), warm accent (#C49A3C)
  Anti-patterns: "not corporate", "not flashy", "not decorative for decoration's sake"

Load: references/brand-to-animation.md
  Personality match: "Quiet / Minimal / Understated"
  Search modifiers: minimal, simple, clean, line, geometric
  Color note: dark background → search for light/outline animations
```

### Step 3: Search LottieFiles

```
WebSearch: site:lottiefiles.com free-animation loading minimal geometric
WebSearch: site:lottiefiles.com free-animation loading simple clean outline

→ Found 6 candidates, WebFetch each page for details
→ Filtered to 4 based on brand coherence (dropped 2 with bright neon colors)
```

Present:
```
Found 4 loading animations matching "Quiet Infrastructure" brand:

1. **Simple Geometric Loader** by Seb Tran
   - Preview: https://lottiefiles.com/free-animation/simple-loader-YjC28wCrYt
   - Colors: Neutral gray, thin lines
   - Size: 929 bytes
   - Brand fit: Clean geometric rotation. Minimal, functional — invisible infrastructure.

2. **Dots Loading** by Raf Tadevossian
   - Preview: https://lottiefiles.com/free-animation/loading-dots-abc123
   - Colors: Monochrome, adaptable
   - Size: 1.2 KB
   - Brand fit: Three subtle pulsing dots. Quiet, rhythmic.

3. **Circle Loader** by Motion Corp
   - Preview: https://lottiefiles.com/free-animation/circle-loader-def456
   - Colors: White outline on transparent
   - Size: 2.1 KB
   - Brand fit: Single circle draw-on animation. Works on dark backgrounds.

4. **Minimal Spinner** by Lottie Studio
   - Preview: https://lottiefiles.com/free-animation/minimal-spinner-ghi789
   - Colors: Light gray
   - Size: 1.8 KB
   - Brand fit: Thin rotating arc. Geometric, understated.

Which would you like to embed? (1-4, or 'refine' to search again)
```

### Step 4: User Selects

User: "3"

### Step 5: Embed

```bash
npm install @lottiefiles/dotlottie-react
```

Create `src/components/LoadingSpinner.tsx`:
```tsx
import { DotLottieReact } from '@lottiefiles/dotlottie-react';

export function LoadingSpinner() {
  return (
    <DotLottieReact
      src="https://lottie.host/def456/circle-loader.lottie"
      autoplay
      loop
      speed={0.75}
      style={{ width: 48, height: 48 }}
    />
  );
}

// Animation: "Circle Loader" by Motion Corp — LottieFiles
```

Note: `speed={0.75}` to match the quiet, unhurried brand energy.

---

## Example 2: Animation Series — UI Feedback Set

**User invokes:** `/lottie-design UI feedback set`

### Step 1-2: Same as above

Detect React + DESIGN.md. Extract brand attributes.

### Step 3: Search for Series

```
Search for loading animation first (anchor for the set):
WebSearch: site:lottiefiles.com free-animation loading minimal geometric

→ Best match: "Minimal UI Loader" by Alex Chen
→ WebFetch creator page: site:lottiefiles.com free-animations/alex-chen

Found 4 more by same creator in matching style:
  - "Minimal Checkmark" (success)
  - "Minimal Cross" (error)
  - "Minimal Warning Triangle" (warning)
  - "Minimal Info Circle" (info)

All share: thin 2px stroke, monochrome, same easing, same size canvas.
```

Present:
```
Found a cohesive UI feedback set from Alex Chen (same creator, matching style):

Set: **Minimal UI Feedback** by Alex Chen
Style: Thin-line, monochrome, geometric — all consistent

  1. Loading → "Minimal UI Loader" (1.1 KB) — rotating arc
  2. Success → "Minimal Checkmark" (0.9 KB) — draw-on check
  3. Error → "Minimal Cross" (0.8 KB) — draw-on X
  4. Warning → "Minimal Warning Triangle" (1.0 KB) — pulsing triangle
  5. Info → "Minimal Info Circle" (0.9 KB) — pulsing i

Coherence: Same line weight, same motion language, same color approach.
Brand fit: Monochrome outlines match "Quiet Infrastructure" — functional, invisible.

Embed this set? (yes / swap individual items / refine search)
```

### Step 4: User Confirms

User: "yes"

### Step 5: Embed Series

```bash
npm install @lottiefiles/dotlottie-react
```

Create `src/animations/index.ts`:
```typescript
export const animations = {
  loading: "https://lottie.host/abc/loader.lottie",
  success: "https://lottie.host/abc/checkmark.lottie",
  error: "https://lottie.host/abc/cross.lottie",
  warning: "https://lottie.host/abc/warning.lottie",
  info: "https://lottie.host/abc/info.lottie",
} as const;

export type AnimationKey = keyof typeof animations;

// All animations by Alex Chen — LottieFiles
```

Create `src/components/LottieAnimation.tsx`:
```tsx
import { DotLottieReact } from '@lottiefiles/dotlottie-react';
import { animations, type AnimationKey } from '@/animations';

interface LottieAnimationProps {
  name: AnimationKey;
  size?: number;
  loop?: boolean;
  speed?: number;
}

export function LottieAnimation({
  name,
  size = 48,
  loop = name === 'loading',
  speed = 0.75,
}: LottieAnimationProps) {
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

Usage:
```tsx
<LottieAnimation name="loading" />
<LottieAnimation name="success" size={64} />
<LottieAnimation name="error" />
```

Note: `loop` defaults to true only for loading. Success/error/warning/info play once.

---

## Example 3: Rive Interactive Component — Like Button

**User invokes:** `/animation-design interactive like button`

### Step 1: Detect Project Context

```
Glob: package.json → found
Read: package.json → "next": "14.x", "react": "18.x" in dependencies
       No @rive-app packages installed yet.

Glob: DESIGN.md → found
Framework: Next.js (React)
Brand context: DESIGN.md present
```

Output: "Detected Next.js (React) project. Found DESIGN.md. User wants interactive component → routing to Rive."

### Step 2: Understand Brand Attributes

Same as Lottie examples — extract mood, palette, aesthetic keywords from DESIGN.md.

### Step 3: Search Rive Marketplace

```
WebSearch: site:rive.app toggle like button interactive
WebSearch: site:rive.app/marketplace like button heart

→ Found 4 candidates, WebFetch each page for details
→ Extracted state machine names, input types, creator info
```

Present:
```
Found 3 interactive like button animations on Rive:

1. **Interactive Like Button Animation** by yaroslavnaa
   - Preview: https://rive.app/marketplace/22667-42414-interactive-like-button-animation/
   - Type: Rive (interactive)
   - State Machine: LikeMachine
   - Inputs: click trigger (toggles between gray/red states)
   - License: CC BY
   - Brand fit: Clean, smooth transitions. Red active state — works on dark backgrounds.

2. **Like Button** by [creator]
   - Preview: https://rive.app/marketplace/1683-3324-like-button/
   - Type: Rive (interactive)
   - State Machine: Toggle
   - Inputs: boolean toggle
   - Brand fit: Minimal, Twitter-style heart toggle.

3. **Subscribe Button** by bijoyChandra
   - Preview: https://rive.app/community/files/18275-34299-/
   - Type: Rive (interactive)
   - State Machine: Subscribe
   - Brand fit: Bell + subscribe animation. More complex.

Which would you like to embed? (1-3, or 'refine' to search again)
```

### Step 4: User Selects

User: "1"

### Step 5: Embed

```bash
npm install @rive-app/react-webgl2
```

Download the `.riv` file:
```bash
mkdir -p public/animations
curl -o public/animations/like-button.riv "https://public.rive.app/community/runtime-files/22667-42414-interactive-like-button-animation.riv"
```

Create `src/components/LikeButton.tsx`:
```tsx
import { useRive, useStateMachineInput } from '@rive-app/react-webgl2';

interface LikeButtonProps {
  size?: number;
  onToggle?: (liked: boolean) => void;
}

export function LikeButton({ size = 48, onToggle }: LikeButtonProps) {
  const { rive, RiveComponent } = useRive({
    src: '/animations/like-button.riv',
    stateMachines: 'LikeMachine',
    autoplay: true,
  });

  return (
    <RiveComponent
      style={{ width: size, height: size, cursor: 'pointer' }}
    />
  );
}

// Animation: "Interactive Like Button Animation" by yaroslavnaa — Rive (CC BY)
```

Note: The state machine handles click interaction internally — no onClick handler needed. Rive's built-in listeners detect pointer events on the canvas.
