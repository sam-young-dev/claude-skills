# CUBE CSS tooling reference

For generating the Utility layer's classes from design tokens (see [SKILL.md](SKILL.md) Layer 2), the CUBE CSS site (cube.fyi/resources) points to:

| Tool | What it does |
| --- | --- |
| [Gorko](https://github.com/hankchizljaw/gorko) | Sass library that generates utility classes from a design-token config |
| [Goron](https://github.com/hankchizljaw/goron) | Same idea as Gorko, but JSON + Node instead of Sass |
| [Tailwind CSS](https://tailwindcss.com) | Utility class generator/framework — CUBE treats it as a compatible utility-generation tool for the Utility layer, not a competing methodology |
| [Style Dictionary](https://amzn.github.io/style-dictionary) | Design token system — transforms one token source into platform-specific outputs (CSS custom properties among them) |
| [Theo](https://github.com/salesforce-ux/theo) | Design token system (Salesforce) |
| [Figgo](https://github.com/B3nnyL/figgo) | Design-token export tool for Figma |

## Further reading

- [cube.fyi](https://cube.fyi) — the official site (Home, Principles, CSS, Composition, Utility, Block, Exception, Grouping, Examples, Resources)
- [piccalil.li/blog/cube-css](https://piccalil.li/blog/cube-css) by Andy Bell — the canonical high-level walkthrough the official site itself points to first
- [Every Layout](https://every-layout.dev) — the layout-primitive approach (`.flow`, `.stack`, `.cluster`, etc.) CUBE's Composition layer draws on and explicitly credits
