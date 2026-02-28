---
name: animation-design
description: Search LottieFiles and Rive Marketplace for brand-coherent animations and embed them into web projects. Use when the user asks to add animations, find Lotties, embed Rive components, search for motion assets, pull in an animation series, or add loading/success/error/interactive animations. Detects brand context from DESIGN.md, CSS variables, or Tailwind config. Routes to Lottie (decorative motion) or Rive (interactive components) based on use case.
version: 2.0.0
tools:
  - WebSearch
  - WebFetch
  - Bash
  - Read
  - Write
  - Glob
  - Grep
---

# Animation Design

Find and embed brand-coherent animations into web projects. Supports Lottie (decorative motion, looping animations) and Rive (interactive components, state machines). Works with single animations and cohesive series.

## Invocation

- `/animation-design loading spinner` — search with intent from `$ARGUMENTS`
- `/animation-design rive toggle button` — explicitly target Rive
- `/animation-design UI feedback set` — find a cohesive Lottie series
- `/animation-design` — no arguments: ask what kind of animation is needed

## Lottie vs Rive — When to Use Which

| Use Case | Recommendation | Why |
|---|---|---|
| Loading spinners, success/error feedback | **Lottie** | Huge library, simple embed, no interactivity needed |
| Decorative hero animations, backgrounds | **Lottie** | Wider selection, CDN-hosted, lightweight |
| Interactive buttons, toggles, switches | **Rive** | State machines handle hover/click/active states |
| Complex multi-state UI components | **Rive** | State machines with triggers, booleans, numbers |
| Animated icons that respond to user input | **Rive** | Built-in interactivity, no JS event wiring |
| Onboarding flows, empty states | **Lottie** | More variety, play-once animations |
| Game-like UI, character animations | **Rive** | Bones, mesh deformation, state machines |

**Default to Lottie** unless the animation needs to respond to user interaction. Lottie has a much larger ecosystem of free assets.

## Workflow

### Step 1: Detect Project Context

Scan the current project to determine framework and brand context.

**Framework detection** — check `package.json` dependencies:
- `react` / `next` → React
- `vue` / `nuxt` → Vue
- `svelte` / `@sveltejs/kit` → Svelte
- None of the above → plain HTML

Check if any animation packages are already installed:
- Lottie: `@lottiefiles/dotlottie-react`, `@lottiefiles/dotlottie-vue`, `@lottiefiles/dotlottie-svelte`, `@lottiefiles/lottie-player`
- Rive: `@rive-app/react-webgl2`, `@rive-app/react-canvas`, `@rive-app/canvas`

**Brand context detection** — look in this priority order:
1. `DESIGN.md` in project root (from brand-skill — richest source)
2. CSS files with `:root` custom properties (Grep for `:root` in `*.css`)
3. `tailwind.config.*` (extract `theme.extend.colors`)
4. Any `design-guidelines.md` or `brand*.md` files

If no brand context is found, note it to the user and proceed with their description only.

Output a brief summary: "Detected Next.js (React) project with DESIGN.md. Ready to search."

### Step 2: Understand Brand Attributes

If brand context was found, extract key signals:

- **Mood/personality** from emotive narrative (e.g., "quiet confidence", "playful energy")
- **Color palette** — dark or light background, warm or cool, muted or vibrant
- **Aesthetic keywords** from visual philosophy (e.g., "minimal", "organic", "brutalist")
- **Anti-patterns** — what the brand is NOT ("not corporate", "not loud")

Load `references/brand-to-animation.md` for the translation table. Map brand attributes to search modifiers.

Combine brand modifiers with the user's intent (`$ARGUMENTS`) to form the search strategy.

### Step 3: Determine Animation Type

Route to Lottie or Rive based on:

1. **Explicit request** — user says "rive" or "lottie" → use that
2. **Interactive intent** — "toggle", "button", "hover state", "interactive" → Rive
3. **Decorative intent** — "loading", "spinner", "background", "empty state" → Lottie
4. **Ambiguous** — present both options with a recommendation

### Step 4: Search

#### Lottie Search (LottieFiles)

**Single animation mode:**
1. Run 2-3 WebSearch queries: `site:lottiefiles.com free-animation {intent} {brand-modifier}`
2. Pick 4-6 promising results
3. WebFetch each animation page to extract: `.lottie` CDN URL, color info, tags, creator name, file size
4. Filter for brand coherence — discard clashing palettes, prefer monochrome/outline for flexibility

**Series mode** (user says "set", "series", "collection", or requests multiple related animations):
1. Search for a strong first match as above
2. Once a good animation is found, search for more by the same creator: `site:lottiefiles.com {creator-name} {category}`
3. Build the set from same-creator animations first (strongest coherence signal)
4. If the creator doesn't cover all needed purposes, search for style-matched alternatives
5. Load `references/lottie-categories.md` for series templates

#### Rive Search (Marketplace)

1. Run 2-3 WebSearch queries: `site:rive.app {intent} {brand-modifier}` and `site:rive.app/marketplace {intent}`
2. WebFetch promising results to check style, state machine complexity, creator
3. Note: Rive marketplace has fewer assets than LottieFiles — may need to broaden search
4. Check creator's other work for series coherence

**Present results:**
```
1. **[Animation Name]** by [Creator]
   - Preview: [page URL]
   - CDN/Download: [assets URL]
   - Type: Lottie / Rive (interactive)
   - Colors: [brief palette description]
   - Size: [X KB]
   - Brand fit: [one sentence assessment]
   - State machines: [if Rive — list state machine names and inputs]
```

For series, present as a grouped set with a coherence note.

### Step 5: User Selects

- Single mode: "Which would you like to embed? (1-5)"
- Series mode: present the set as a group, allow swapping individual items
- Offer to refine search with different terms if nothing fits

### Step 6: Embed

Load `references/embedding-patterns.md` (Lottie) or `references/rive-embedding-patterns.md` (Rive) for exact code patterns.

#### Lottie Install

1. Install the appropriate package if not present:
   - React: `npm install @lottiefiles/dotlottie-react`
   - Vue: `npm install @lottiefiles/dotlottie-vue`
   - Svelte: `npm install @lottiefiles/dotlottie-svelte`
   - HTML: CDN script tag for `@lottiefiles/lottie-player@2.0.3` (NOT dotlottie-wc)

#### Rive Install

1. Install the appropriate package if not present:
   - React: `npm install @rive-app/react-webgl2`
   - Vue: `npm install @rive-app/canvas`
   - Svelte: `npm install @rive-app/canvas`
   - HTML: CDN script tag via unpkg (`@rive-app/canvas`)

2. For Rive: download the `.riv` file and place in `public/animations/` (no public CDN — must self-host)

#### Common Steps

3. Generate component(s) with the selected animation's URL or local path.

4. For series, create a shared animations config:
   ```typescript
   // animations.ts
   export const animations = {
     loading: "/animations/loader.riv",      // or .lottie CDN URL
     success: "/animations/checkmark.riv",
     error: "/animations/cross.riv",
   } as const;
   ```
   Plus a reusable wrapper component that takes a purpose key.

5. Use the project's spacing system for sizing (CSS variables, Tailwind classes, or explicit px).

6. Note creator attribution as a code comment.

## Animation Coherence Guidelines

**Motion matches energy.** A "quiet confidence" brand needs subtle, slow animations. A "playful" brand can use bouncy, exaggerated motion. Adjust with the `speed` prop (0.5 for slower, 2 for faster).

**Color compatibility.** Both Lottie and Rive animations have baked-in colors. Filter search results for palette fit. Prefer monochrome/outline animations for maximum flexibility across themes.

**Purpose over decoration.** Every animation communicates something: loading states, success confirmations, empty states, onboarding, transitions. "Add an animation because it feels empty" is not a reason — what is the animation communicating?

**Series consistency.** When embedding multiple animations, they must feel like they came from the same designer. Same line weight, same motion language, same color approach. Prioritize same-creator animations.

**Interactive vs decorative.** Don't use Rive for a simple looping spinner — that's Lottie territory. Don't use Lottie for a button that needs hover/active/disabled states — that's Rive territory. Match the tool to the job.

## Reference Files

- `references/embedding-patterns.md` — Lottie framework-specific code snippets, speed/event control, series barrel pattern
- `references/rive-embedding-patterns.md` — Rive framework-specific code snippets, state machine control, series pattern
- `references/brand-to-animation.md` — brand personality → search modifier translation tables, Lottie vs Rive selection
- `references/lottie-categories.md` — common categories, series templates, alternative keywords, Rive marketplace notes
- `examples/workflow-example.md` — end-to-end Lottie single + series example, Rive interactive component example

## Technical Notes

### Lottie
- **Plain HTML**: Use `@lottiefiles/lottie-player@2.0.3` (NOT `dotlottie-wc` — it breaks on live domains/Vercel deploys). Use `<lottie-player>` tag with self-hosted JSON files extracted from `.lottie` zips. See embedding-patterns.md for details.
- **React/Vue/Svelte**: The `@lottiefiles/dotlottie-react` / `dotlottie-vue` / `dotlottie-svelte` packages work fine in bundled frameworks. These can use `.lottie` format directly.
- `lottie-web` is deprecated — don't use it.
- CDN URLs from `lottie.host` or `assets-v2.lottiefiles.com` are directly embeddable for framework packages.
- For plain HTML deploys: download `.lottie` files, unzip to extract the JSON, self-host in a `/lotties/json/` directory. `.lottie` files are just zips containing JSON + assets.

### Rive
- **All frameworks**: Use `@rive-app/canvas` (base) or `@rive-app/react-webgl2` (React). No dedicated Vue or Svelte packages — use canvas runtime directly.
- **No public CDN** for `.riv` files. Always self-host in `public/animations/` or similar.
- **State machines** are the key differentiator — they handle interactivity without custom JS event code.
- Always call `resizeDrawingSurfaceToCanvas()` in the `onLoad` callback for crisp rendering on high-DPI displays.
- `.riv` files are binary (not JSON). Typically 10-15x smaller than equivalent Lottie JSON.
- Rive marketplace has fewer free assets than LottieFiles — may need to broaden searches.

### General
- Fetch animation pages sequentially (not parallel) to avoid rate limiting. Limit to 5-6 page fetches per search.
- If WebFetch fails on a specific page, use the URL from search results directly.
