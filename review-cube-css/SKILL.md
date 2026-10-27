---
name: review-cube-css
description: Review existing CSS and HTML against the CUBE CSS methodology and report violations as a table. Use when asked to review, audit or refactor a stylesheet for CUBE consistency, or to check a diff for layer mistakes. For writing new CSS, use cube-css.
---

# Review CUBE CSS

Read-only. Report findings. Never edit unless asked. For the rules and the reasoning behind them, load `cube-css`.

## Output format

Always a single markdown table. One row per issue.

| Location | Problem | Fix | Why |
| --- | --- | --- | --- |
| `card.css:14` | `.card--reversed` modifier class | `.card[data-state='reversed']` | Variants are exceptions; JS and CSS share the attribute |

Order rows by severity: wrong layer first, then specificity hacks, then size, then naming and grouping. If nothing violates, say so in one line. Never pad the table.

## Checks

Run every check on every file.

| Check | Violation signal | Fix |
| --- | --- | --- |
| Global baseline | Page looks broken without class-based CSS; resets or typography set in classes | Move to element and `:root` selectors |
| Composition purity | Layout class sets colour, font, shadow or pattern | Move decoration to a block or utility |
| Composition rigidity | Fixed pixel widths and heights on layout classes | Use flexible rules the browser can adapt |
| Flow | Per-component `margin-bottom` or `margin-top` rules between siblings | Use `.flow` and `--flow-space` |
| Utility scope | One class bundles unrelated properties | Promote to a block |
| Utility source | Hand-written token values duplicated in several classes | Generate from the token source |
| Orphan utilities | A class in HTML with no CSS rule | Generate the missing class or remove it |
| Specificity hacks | `!important`, ID selectors, selectors deeper than block + one descendant | Flatten to one class per layer |
| Block size | A block over roughly 100 lines | Split, or move rules up a layer |
| Block focus | One file or block styles more than one component | Split into one block per component |
| Block redundancy | Block re-declares what global, composition or utility already do | Delete the duplicate |
| Exception form | Modifier class (`--x`, `.is-x`) for a state or variant | `data-*` attribute selector |
| Exception size | An exception overrides most of the block | Make it a new block |
| Class order | Classes in the wrong order | Block, other blocks, utilities, token utilities |
| Class delimiters | `[ ]`, `\|` or other delimiters between class groups in HTML, JSX or templates | Remove them: `card section box bg-base` |
| Naming consistency | Mixed `.block .el` and `.block__el` in one project | Pick one and convert |

## Rules for the review

- Never flag a choice the methodology leaves open, such as descendant vs BEM-style naming. Flag only inconsistency within the project.
- Always flag delimiters (square brackets, pipes) between class groups. Classes are plain and space-separated.
- Never flag a tool choice. Sass, PostCSS, CSS-in-JS and Tailwind-generated utilities are all valid.
- Cite file and line for every row.
- Every Fix must be concrete CSS or HTML, not advice.
