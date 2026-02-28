# Claude Lottie Skill

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that searches [LottieFiles](https://lottiefiles.com) for brand-coherent Lottie animations and embeds them into web projects.

## What It Does

Invoke `/lottie-design` in Claude Code and it will:

1. **Detect your project** — framework (React, Vue, Svelte, plain HTML) and brand context (from `DESIGN.md`, CSS custom properties, or Tailwind config)
2. **Translate brand to motion** — map your brand personality (minimal, playful, luxury, etc.) to LottieFiles search modifiers
3. **Search LottieFiles** — find animations that match both your intent and your brand
4. **Present options** — show 4-6 candidates with preview links, file sizes, and brand-fit assessments
5. **Embed** — install the right `@lottiefiles/dotlottie-*` package and generate framework-specific components

### Single Animation

```
/lottie-design loading spinner
```

Searches for a loading animation that matches your brand, presents options, embeds your pick.

### Animation Series

```
/lottie-design UI feedback set
```

Finds a cohesive set (loading, success, error, warning, info) from the same creator or with matching visual style. Generates a barrel file and reusable wrapper component.

## Installation

Copy `SKILL.md` to your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/lottie-design
cp SKILL.md ~/.claude/skills/lottie-design/SKILL.md
cp -r references ~/.claude/skills/lottie-design/references
cp -r examples ~/.claude/skills/lottie-design/examples
```

The skill requires these Claude Code tools: `WebSearch`, `WebFetch`, `Bash`, `Read`, `Write`, `Glob`, `Grep`.

## Supported Frameworks

| Framework | Package | Install Required |
|-----------|---------|-----------------|
| React / Next.js | `@lottiefiles/dotlottie-react` | Yes |
| Vue / Nuxt | `@lottiefiles/dotlottie-vue` | Yes |
| Svelte / SvelteKit | `@lottiefiles/dotlottie-svelte` | Yes |
| Plain HTML | `@lottiefiles/dotlottie-wc` (CDN) | No |

## Brand Detection

The skill looks for brand context in this order:

1. `DESIGN.md` in project root (richest source — from [claude-brand-skills](https://github.com/b1rdmania/claude-brand-skills))
2. CSS files with `:root` custom properties
3. `tailwind.config.*` color extensions
4. Any `design-guidelines.md` or `brand*.md` files

If no brand context is found, it proceeds with your description only.

## Reference Files

| File | Purpose |
|------|---------|
| `references/brand-to-animation.md` | Brand personality to search modifier translation tables |
| `references/embedding-patterns.md` | Framework-specific code snippets, props reference, series patterns |
| `references/lottie-categories.md` | Common categories, series templates, alternative search strategies |
| `examples/workflow-example.md` | End-to-end walkthrough for single and series embedding |

## How It Works

The skill maps brand attributes to animation search strategies:

| Brand Personality | Animation Style | Search Modifiers |
|---|---|---|
| Quiet / Minimal | Subtle, geometric, thin-line | `minimal`, `clean`, `line` |
| Warm / Organic | Smooth curves, hand-drawn | `organic`, `smooth`, `natural` |
| Bold / Playful | Bouncy, exaggerated | `bouncy`, `fun`, `dynamic` |
| Luxury / Elegant | Slow, smooth, metallic | `elegant`, `premium`, `gold` |
| Technical / Precise | Data viz, mechanical | `data`, `tech`, `circuit` |

For animation series, it prioritises same-creator animations (strongest coherence signal) before falling back to style-matched alternatives.

## Technical Notes

- Uses only `@lottiefiles/dotlottie-*` packages (modern, WASM-based). `lottie-web` is deprecated.
- Prefers `.lottie` (dotLottie) format over `.json` — up to 80% smaller files.
- CDN URLs from `lottie.host` are directly embeddable.
- Fetches animation pages sequentially to avoid rate limiting (max 5-6 per search).
