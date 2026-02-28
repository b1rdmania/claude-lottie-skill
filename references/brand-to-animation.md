# Brand-to-Animation Translation

Map brand attributes to LottieFiles search strategies. Read the project's DESIGN.md or design system, identify the brand personality, then use the tables below to construct targeted search queries.

## By Brand Personality

| Brand Personality | Animation Style | Search Modifiers | Avoid |
|---|---|---|---|
| Quiet / Minimal / Understated | Subtle, geometric, thin-line | `minimal`, `simple`, `clean`, `line`, `geometric` | Bouncy, colorful, complex, 3D |
| Warm / Organic / Human | Smooth curves, hand-drawn feel, natural | `organic`, `smooth`, `hand-drawn`, `soft`, `natural` | Sharp, mechanical, glitchy |
| Bold / Energetic / Playful | Bouncy, exaggerated, colorful | `bouncy`, `fun`, `colorful`, `dynamic`, `cartoon` | Subtle, muted, slow |
| Technical / Precise / Systematic | Data visualization, mechanical, structured | `data`, `tech`, `system`, `code`, `dashboard`, `circuit` | Organic, hand-drawn, casual |
| Luxury / Refined / Elegant | Slow, smooth, gold/metallic, subtle | `elegant`, `premium`, `smooth`, `gold`, `luxury` | Cartoon, bouncy, loud |
| Brutalist / Raw / Experimental | Glitch, distortion, stark, high-contrast | `glitch`, `abstract`, `experimental`, `distort` | Polished, smooth, corporate |
| Friendly / Approachable | Rounded, soft, character-based | `friendly`, `cute`, `character`, `rounded`, `smile` | Angular, cold, technical |
| Infrastructure / Invisible | Functional, understated, purposeful | `minimal`, `utility`, `functional`, `simple`, `thin` | Decorative, flashy, attention-grabbing |

## By Animation Purpose

| Use Case | Primary Search Terms | Secondary Terms | Notes |
|---|---|---|---|
| Loading / Spinner | `loading`, `spinner`, `loader` | `dots`, `circle`, `progress` | Most common need |
| Success / Complete | `success`, `checkmark`, `done`, `complete` | `confetti` (playful), `check` (minimal) | Play once, don't loop |
| Error / Failure | `error`, `warning`, `alert`, `fail` | `cross`, `x-mark`, `exclamation` | Brief, not alarming |
| Empty State | `empty`, `no-data`, `not-found` | `search`, `box`, `desert` | Gentle, inviting |
| 404 / Not Found | `404`, `not-found`, `lost`, `missing` | `astronaut`, `space` (common motif) | Can be more playful |
| Onboarding / Welcome | `welcome`, `onboarding`, `intro` | `arrow`, `guide`, `tutorial`, `wave` | Should feel warm |
| Hero / Decorative | `abstract`, `shape`, `particle`, `wave` | `gradient`, `morph`, `flow` | Match brand palette |
| Scroll / Transition | `scroll`, `transition`, `reveal` | `arrow-down`, `wave`, `slide` | Subtle, directional |
| Upload / Progress | `upload`, `progress`, `file` | `cloud`, `arrow-up`, `transfer` | Show clear progress |
| Download | `download`, `file`, `save` | `arrow-down`, `cloud` | Brief confirmation |
| Chat / Message | `chat`, `message`, `typing` | `bubble`, `notification`, `dots` | Typing indicator |
| Like / Favorite | `heart`, `like`, `favorite` | `thumbs-up`, `star`, `love` | Play once on action |
| Toggle / Switch | `toggle`, `switch`, `on-off` | `checkbox`, `radio` | Two-state animation |
| Menu / Nav | `hamburger`, `menu`, `navigation` | `close`, `arrow`, `back` | Transition between states |
| Send / Submit | `send`, `submit`, `mail` | `paper-plane`, `envelope`, `rocket` | Play once on action |
| Refresh / Sync | `refresh`, `sync`, `reload` | `rotate`, `update`, `circular` | Loop while active |
| Delete / Trash | `delete`, `trash`, `remove` | `bin`, `shred`, `disappear` | Brief, confirmatory |
| Settings / Gear | `settings`, `gear`, `config` | `cog`, `wrench`, `tool` | Subtle rotation |

## By Color Compatibility

### Dark backgrounds
- Search for animations with light/white elements
- Add `white`, `light`, or `outline` to search terms
- Monochrome line animations work best
- Avoid animations with dark fills that disappear against the background

### Light backgrounds
- Most animations work on light backgrounds
- Avoid white-dominant or very light animations
- Bold colors and filled shapes read well

### Monochrome / Flexible
- Add `outline`, `line-art`, `mono`, `wireframe` to search terms
- These adapt to any palette since they rely on stroke, not fill
- Best choice when the brand palette is unusual or high-contrast

### Warm palettes (amber, orange, earth tones)
- Search for `warm`, `orange`, `earth`, `natural`
- Avoid cool blues and purples that clash with warm schemes

### Cool palettes (blue, teal, purple)
- Search for `blue`, `cool`, `tech`, `digital`
- Avoid warm oranges and yellows that create dissonance

## Constructing Search Queries

Combine intent + brand modifier + color hint:

```
site:lottiefiles.com free-animation {intent} {brand_modifier}
```

Examples:
- Quiet brand needs loading: `site:lottiefiles.com free-animation loading minimal geometric`
- Playful brand needs success: `site:lottiefiles.com free-animation success confetti bouncy`
- Dark-themed luxury brand needs hero: `site:lottiefiles.com free-animation abstract elegant gold`
- Technical brand needs empty state: `site:lottiefiles.com free-animation empty state minimal tech`

Run 2-3 variations per search. If first round yields nothing good, drop the brand modifier and search with intent only, then filter results manually for brand fit.

## Series Coherence Signals

When building a series, check these consistency markers:

1. **Same creator** — strongest signal. Same person = same visual language.
2. **Same line weight** — thin-line animations should all be thin-line.
3. **Same fill approach** — all outline OR all filled, not mixed.
4. **Same motion style** — all ease-in-out OR all bouncy, not mixed.
5. **Same color count** — all monochrome OR all 2-color, not mixed.
6. **Same level of detail** — all simple OR all detailed, not mixed.

When browsing a creator's page for series candidates, check their other animations for matching style. A creator who makes a good loading animation likely has success/error/empty in the same visual language.

## Lottie vs Rive Selection

Choose the animation technology based on what the animation needs to do:

| Signal | → Lottie | → Rive |
|---|---|---|
| Loops forever, no user interaction | Yes | Overkill |
| Responds to hover, click, or drag | Needs custom JS | Built-in state machines |
| Simple feedback (success, error, loading) | Best choice — huge library | Fewer assets available |
| Multi-state component (toggle, switch, button) | Poor fit | Built for this |
| Decorative background/hero animation | Best choice | Can work, fewer options |
| Needs to change based on data (progress, score) | Manual JS wiring | Number inputs in state machine |
| File size critical | JSON can be large | Binary .riv is 10-15x smaller |
| Must work without JS bundler (plain HTML) | Excellent — lottie-player web component | Requires canvas + JS setup |

### Rive Search Modifiers

The same brand personality modifiers work for Rive searches, but the ecosystem is smaller. When searching Rive:

- Use `site:rive.app {intent} {brand-modifier}` and `site:rive.app/marketplace {intent}`
- Rive marketplace has fewer assets — broaden terms if first search yields little
- Check `site:rive.app/community` for free community uploads
- Rive assets often include state machines — note the state machine names and inputs when presenting results
- Runtime `.riv` files are available at: `https://public.rive.app/community/runtime-files/{id}.riv`
