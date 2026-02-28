# Lottie Categories & Series Templates

## Common LottieFiles Categories

### UI / UX
`loading`, `spinner`, `toggle`, `menu`, `hamburger`, `scroll`, `swipe`, `tab`, `switch`, `checkbox`, `radio`, `slider`, `tooltip`, `modal`, `dropdown`, `pagination`, `breadcrumb`

### Feedback
`success`, `error`, `warning`, `info`, `notification`, `toast`, `alert`, `confirm`, `checkmark`, `cross`

### Content States
`empty-state`, `no-results`, `404`, `maintenance`, `coming-soon`, `offline`, `no-connection`, `under-construction`

### Actions
`download`, `upload`, `send`, `share`, `like`, `bookmark`, `copy`, `paste`, `delete`, `trash`, `edit`, `save`, `print`, `refresh`, `sync`

### Social
`heart`, `thumbs-up`, `star`, `emoji`, `reaction`, `comment`, `follow`, `subscribe`

### Data / Charts
`chart`, `graph`, `analytics`, `dashboard`, `statistics`, `pie-chart`, `bar-chart`, `line-graph`, `counter`

### Navigation
`arrow`, `back`, `forward`, `scroll-down`, `chevron`, `expand`, `collapse`, `close`, `home`

### Weather
`sun`, `rain`, `cloud`, `snow`, `thunder`, `wind`, `fog`, `rainbow`, `temperature`

### Characters / Illustration
`character`, `person`, `animal`, `robot`, `mascot`, `avatar`, `walk-cycle`, `wave`, `thinking`

### Business
`payment`, `money`, `wallet`, `card`, `invoice`, `receipt`, `shopping-cart`, `delivery`, `shipping`

### Communication
`email`, `chat`, `message`, `phone`, `video-call`, `microphone`, `speaker`, `bell`

## Series Templates

Standard animation sets to search for as a cohesive group. When a user requests a "series", "set", or "collection", use these templates to know which purposes to fill.

### UI Feedback Set (most common)
| Purpose | Search Terms | Play Behavior |
|---|---|---|
| Loading | `loading`, `spinner` | Loop |
| Success | `success`, `checkmark` | Play once |
| Error | `error`, `cross` | Play once |
| Warning | `warning`, `alert` | Play once |
| Info | `info`, `information` | Play once |

### Content State Set
| Purpose | Search Terms | Play Behavior |
|---|---|---|
| Empty | `empty-state`, `no-data` | Loop (gentle) |
| No Results | `no-results`, `search-empty` | Loop (gentle) |
| 404 | `404`, `not-found` | Loop |
| Offline | `no-connection`, `offline` | Loop |
| Maintenance | `maintenance`, `under-construction` | Loop |

### Action Set
| Purpose | Search Terms | Play Behavior |
|---|---|---|
| Download | `download`, `arrow-down` | Play once |
| Upload | `upload`, `arrow-up` | Play once |
| Send | `send`, `paper-plane` | Play once |
| Delete | `delete`, `trash` | Play once |
| Save | `save`, `bookmark` | Play once |

### Onboarding Set
| Purpose | Search Terms | Play Behavior |
|---|---|---|
| Welcome | `welcome`, `wave`, `hello` | Play once |
| Step Indicator | `progress`, `steps`, `dots` | Loop |
| Feature Highlight | `spotlight`, `highlight`, `new` | Loop (gentle) |
| Complete | `celebration`, `confetti`, `done` | Play once |

### Social Interaction Set
| Purpose | Search Terms | Play Behavior |
|---|---|---|
| Like | `heart`, `like` | Play once |
| Comment | `chat`, `comment` | Play once |
| Share | `share`, `send` | Play once |
| Subscribe | `bell`, `notification` | Play once |
| Bookmark | `bookmark`, `save` | Play once |

## Alternative Search Strategies

When the first round of searching doesn't yield good results, try these angles:

### Broaden
- Drop brand modifiers, search intent only: `site:lottiefiles.com free-animation loading`
- Use more generic terms: "animation" instead of specific type
- Try the category name: `site:lottiefiles.com free-animation ui` or `site:lottiefiles.com free-animation icon`

### Narrow
- Add format descriptor: `outline`, `filled`, `3d`, `flat`, `isometric`
- Add color: `blue`, `green`, `dark`, `white`, `monochrome`
- Add style: `minimalist`, `cartoon`, `hand-drawn`, `geometric`

### Pivot
- Search by visual metaphor instead of literal purpose: "hourglass" instead of "loading", "rocket" instead of "sending"
- Search by art style: `line art animation`, `flat design animation`, `isometric animation`
- Search by creator: once you find one good animation, search `site:lottiefiles.com {creator-name}` to see their full catalog

### Browse by Collection
- LottieFiles has curated collections. Try: `site:lottiefiles.com collection {style}`
- Some creators publish matching sets. Once a creator match is found, browse their profile for the full set.

## Rive Marketplace

Rive has a smaller but growing marketplace at `rive.app/marketplace/` and community uploads at `rive.app/community/`.

### Rive Categories

Rive assets tend to be interactive components rather than decorative animations:

**Interactive UI**
`toggle`, `switch`, `button`, `checkbox`, `radio`, `slider`, `like`, `favorite`, `subscribe`, `menu`, `hamburger`

**Feedback**
`loading`, `spinner`, `success`, `error`, `checkmark`, `progress`

**Characters**
`avatar`, `character`, `mascot`, `emoji`, `reaction`

**Icons**
`animated-icon`, `icon-set`, `arrow`, `close`, `search`

### Rive Search Strategy

```
site:rive.app/marketplace {intent} {style-modifier}
site:rive.app/community {intent}
site:rive.app {intent} interactive
```

- Marketplace has curated, higher-quality assets (some premium)
- Community has free uploads, wider variety, less curation
- Runtime `.riv` files available at: `https://public.rive.app/community/runtime-files/{id}.riv`
- Check the state machine names and input types on each page — these determine what interactivity is available
- License is typically CC BY — note creator for attribution
