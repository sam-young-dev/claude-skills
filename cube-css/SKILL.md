---
name: cube-css
description: Write CSS using the CUBE CSS methodology (Composition, Utility, Block, Exception) by Andy Bell and Ben Smith. Use when writing or structuring a stylesheet, naming classes, choosing between a utility, a block, or a state variant, adding layout or spacing rules, or grouping classes in HTML. For auditing existing CSS against CUBE, use review-cube-css.
---

# CUBE CSS

Do the styling at the highest layer that can express it. Drop a layer only when the layer above cannot. Fewer rules at the bottom means less CSS overall.

Reason: CSS is forgiving and cascades. Work with the cascade and inheritance instead of fighting them with specificity. By the time a rule reaches a block, most of the work is already done upstream.

Tool-agnostic. Sass, PostCSS, CSS-in-JS and plain CSS all work. Only the CSS output matters.

## Layer decision tree

Answer in order. Stop at the first yes.

1. Can an element selector, `:root` rule or custom property express it? → **Global** (Layer 0)
2. Is it layout, or spacing between elements? → **Composition** (Layer 1)
3. Is it one property, or a tight group of related properties, reusable across unrelated components? → **Utility** (Layer 2)
4. Is it specific to one component's context? → **Block** (Layer 3)
5. Is it a state or variant of an existing block? → **Exception** (Layer 4)

Never write a block rule for something layers 0-2 can do. Reason: a block rule is more specific, harder to override, and duplicates upstream work.

## Layer 0: Global

Element defaults, resets, base typography and colour, and design tokens as custom properties. Use element and `:root` selectors only.

- A page with only global CSS loaded must already look acceptable.
- Everything below progressively enhances this baseline. It never compensates for a missing one.

Reason: global styles cost nothing in specificity and apply everywhere without markup.

## Layer 1: Composition

Flexible layout and rhythm. Hint the browser with flexible rules. Never micro-manage with pixel-exact ones, because the browser knows the context.

A composition class:
- Always handles layout and the spacing between children.
- Never sets colour, font styling, shadows or patterns.
- Never assumes which component sits inside it.

Reason: a component-agnostic skeleton lets any block, or any variant of a block, sit in it without breaking the layout.

Flagship primitive, flow:

```css
.flow > * + * {
  margin-top: var(--flow-space, 1em);
}
```

Override spacing by setting the custom property in context. Never write a competing margin rule.

```css
.card__content {
  --flow-space: 1.4rem;
}
```

Use composition inside blocks too, e.g. `[ card__content ] [ flow ]`. It is not only for page layout. Flow is near-identical to the Stack layout in Every Layout.

## Layer 2: Utility

One class, one job. A single property or a small group of related properties.

```css
.wrapper {
  margin-inline: auto;
  padding-inline: 1rem;
  max-width: 60rem;
}
```

A utility:
- Always extends a design token, so the token stays the single source of truth.
- Always moves repeatable styling into the HTML.
- Never bundles unrelated properties. `color` + `font-size` + `padding` in one class is a block.
- Never acts as a specificity hack. `!important` on every utility breaks down over time.

Generate token utilities from the token source. Never hand-write them.

```css
.bg-primary { background: #ff00ff; }
.color-primary { color: #ff00ff; }
```

Every token used in HTML needs a generated class. A `color-base` class in markup with no matching rule does nothing.

Tools: see [RESOURCES.md](RESOURCES.md).

## Layer 3: Block

A skeletal component: card, button. Light by design.

A block:
- Extends the global, composition and utility work. It never redoes it.
- Applies a small group of design tokens.
- Acts as a namespace. The one extra class is one extra specificity point, and that is enough.
- Always solves one contextual problem. A card and a button in one file are two blocks.
- Never exceeds roughly 80-100 lines. Past that, split it or move rules up into composition or utility.
- Uses composition for its own content flow.

Reason: BEM styles everything inside the block. CUBE does not, because the layers above already did.

Element naming is optional. Pick one per project and use it everywhere.

```css
/* descendant */
.my-block .content { }

/* BEM-style */
.my-block__content { }
```

Descendant selectors on elements are fine too: `.my-block img`.

## Layer 4: Exception

A state-driven deviation from a block. Always a `data-*` attribute. Never a modifier class.

```html
<article class="card" data-state="reversed"></article>
```

```css
.card[data-state='reversed'] {
  display: flex;
  flex-direction: column-reverse;
}
```

Reasons:
- A data attribute signals the variation is exceptional, not part of the block's normal API.
- CSS and JS read the same attribute. JS sets it, CSS reacts.
- It fits finite-state-machine thinking: one attribute, discrete values.

Naming: attribute names the category (`data-state`), value names the state (`reversed`, `inactive`).

If an exception overrides most of the block, make a new block. Reason: the variant has become a different component.

## Grouping classes

Group classes in the `class` attribute so the layers stay readable. Order: primary block, other blocks, standard utilities, token utilities.

```html
<article
  class="[ card ] [ section box ] [ bg-base color-primary ]"
  data-state="reversed"
></article>
```

Brackets and pipes (`card | section box | bg-base color-primary`) are both valid. Pick one per project. Consistency beats the choice of delimiter.

## Bad / Good

| Bad | Good | Why |
| --- | --- | --- |
| `.card.card--reversed { }` | `.card[data-state='reversed'] { }` | Variants are exceptions, and JS can drive the attribute |
| `.card p { margin-bottom: 1rem }` repeated per block | `.flow` on the container | Spacing is composition, set once |
| `.card-title { color; font-size; padding }` as a utility | Block rule, or tokens in the block | Unrelated properties make it a block |
| `.btn { color: red !important }` | Fix source order or use one flat class | `!important` is a specificity hack |
| Hand-written `.bg-primary` | Generated from the token file | One source of truth |
| 200-line `.card { }` | Split, or push rules into flow and utilities | Blocks stay light |

## Working a task

1. Run the layer decision tree before writing any selector.
2. Reuse existing composition and utility classes before adding any.
3. Match the project's existing choices for element naming and grouping delimiter. If none exist, use descendant naming and brackets.
4. Group the classes on every element you touch.
