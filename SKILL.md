---
name: lottie-design
description: Search LottieFiles for brand-coherent Lottie animations and embed them into web projects. Use when the user asks to add animations, find Lotties, embed motion, search LottieFiles, pull in an animation series, or add a loading/success/error animation. Detects brand context from DESIGN.md, CSS variables, or Tailwind config.
version: 1.0.0
tools:
  - WebSearch
  - WebFetch
  - Bash
  - Read
  - Write
  - Glob
  - Grep
---

# Lottie Design

Find and embed brand-coherent Lottie animations into web projects. Supports single animations and cohesive series (loading, success, error, empty state, etc.).

## Invocation

- `/lottie-design loading spinner` — search with intent from `$ARGUMENTS`
- `/lottie-design UI feedback set` — find a cohesive series
- `/lottie-design` — no arguments: ask what kind of animation is needed

## Workflow

### Step 1: Detect Project Context

Scan the current project to determine framework and brand context.

**Framework detection** — check `package.json` dependencies:
- `react` / `next` → React
- `vue` / `nuxt` → Vue
- `svelte` / `@sveltejs/kit` → Svelte
- None of the above → plain HTML

Check if `@lottiefiles/dotlottie-react`, `@lottiefiles/dotlottie-vue`, `@lottiefiles/dotlottie-svelte`, or `@lottiefiles/dotlottie-wc` is already installed.

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

Load `references/brand-to-animation.md` for the translation table. Map brand attributes to LottieFiles search modifiers.

Combine brand modifiers with the user's intent (`$ARGUMENTS`) to form the search strategy.

### Step 3: Search LottieFiles

Two modes depending on user intent:

**Single animation mode:**
1. Run 2-3 WebSearch queries: `site:lottiefiles.com {intent} {brand-modifier}`
2. Pick 4-6 promising results
3. WebFetch each animation page to extract: `.lottie` CDN URL, color info, tags, creator name, file size
4. Filter for brand coherence — discard clashing palettes, prefer monochrome/outline for flexibility

**Series mode** (user says "set", "series", "collection", or requests multiple related animations):
1. Search for a strong first match as above
2. Once a good animation is found, search for more by the same creator: `site:lottiefiles.com {creator-name} {category}`
3. Build the set from same-creator animations first (strongest coherence signal)
4. If the creator doesn't cover all needed purposes, search for style-matched alternatives: same line weight, same motion approach
5. Load `references/lottie-categories.md` for series templates defining standard sets

**Present results:**
```
1. **[Animation Name]** by [Creator]
   - Preview: [LottieFiles page URL]
   - CDN: [assets URL]
   - Colors: [brief palette description]
   - Size: [X KB]
   - Brand fit: [one sentence assessment]
```

For series, present as a grouped set with a coherence note.

### Step 4: User Selects

- Single mode: "Which would you like to embed? (1-5)"
- Series mode: present the set as a group, allow swapping individual items
- Offer to refine search with different terms if nothing fits
- Load `references/lottie-categories.md` for alternative search angles

### Step 5: Embed

Load `references/embedding-patterns.md` for exact code patterns.

1. Install the appropriate package if not present:
   - React: `npm install @lottiefiles/dotlottie-react`
   - Vue: `npm install @lottiefiles/dotlottie-vue`
   - Svelte: `npm install @lottiefiles/dotlottie-svelte`
   - HTML: CDN script tag for `@lottiefiles/lottie-player@2.0.3` (NOT dotlottie-wc)

2. Generate component(s) with the selected animation's CDN URL.

3. For series, create a shared animations config:
   ```typescript
   // animations.ts
   export const animations = {
     loading: "https://lottie.host/...",
     success: "https://lottie.host/...",
     error: "https://lottie.host/...",
     empty: "https://lottie.host/...",
   } as const;
   ```
   Plus a reusable wrapper component that takes a purpose key.

4. Use the project's spacing system for sizing (CSS variables, Tailwind classes, or explicit px).

5. Note creator attribution as a code comment.

## Animation Coherence Guidelines

**Motion matches energy.** A "quiet confidence" brand needs subtle, slow animations. A "playful" brand can use bouncy, exaggerated motion. Adjust with the `speed` prop (0.5 for slower, 2 for faster).

**Color compatibility.** Lottie animations have baked-in colors. Filter search results for palette fit. Prefer monochrome/outline animations for maximum flexibility across themes. Programmatic recoloring is out of scope (requires the LottieFiles editor).

**Purpose over decoration.** Every animation communicates something: loading states, success confirmations, empty states, onboarding, transitions. "Add an animation because it feels empty" is not a reason — what is the animation communicating?

**Series consistency.** When embedding multiple animations, they must feel like they came from the same designer. Same line weight, same motion language, same color approach. Prioritize same-creator animations over style-matching from different creators.

## Reference Files

- `references/embedding-patterns.md` — framework-specific code snippets, speed/event control, series barrel pattern
- `references/brand-to-animation.md` — brand personality → search modifier translation tables
- `references/lottie-categories.md` — common categories, series templates, alternative keywords
- `examples/workflow-example.md` — end-to-end single + series example

## Technical Notes

- **Plain HTML**: Use `@lottiefiles/lottie-player@2.0.3` (NOT `dotlottie-wc` — it breaks on live domains/Vercel deploys). Use `<lottie-player>` tag with self-hosted JSON files extracted from `.lottie` zips. See embedding-patterns.md for details.
- **React/Vue/Svelte**: The `@lottiefiles/dotlottie-react` / `dotlottie-vue` / `dotlottie-svelte` packages work fine in bundled frameworks. These can use `.lottie` format directly.
- `lottie-web` is deprecated — don't use it.
- CDN URLs from `lottie.host` or `assets-v2.lottiefiles.com` are directly embeddable for framework packages.
- For plain HTML deploys: download `.lottie` files, unzip to extract the JSON, self-host in a `/lotties/json/` directory. `.lottie` files are just zips containing JSON + assets.
- Fetch animation pages sequentially (not parallel) to avoid rate limiting. Limit to 5-6 page fetches per search.
- If WebFetch fails on a specific page, use the URL from search results directly.
