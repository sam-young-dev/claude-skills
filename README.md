# claude-skills

Agent skills for Claude Code, by Sam Young.

## Skills

| Skill | Use it to |
| --- | --- |
| [cube-css](cube-css/SKILL.md) | Write CSS with the CUBE CSS methodology (Composition, Utility, Block, Exception) |
| [review-cube-css](review-cube-css/SKILL.md) | Audit existing CSS and HTML against CUBE CSS and get a findings table |

## Install

Personal (all projects):

```sh
git clone https://github.com/<your-username>/claude-skills.git
cp -r claude-skills/cube-css claude-skills/review-cube-css ~/.claude/skills/
```

Single project: copy the folders into that project's `.claude/skills/` instead.

Invoke with `/cube-css` or `/review-cube-css`, or let Claude pick them up from the task.

## Attribution

These skills summarize and restructure the CUBE CSS methodology created by [Andy Bell](https://piccalil.li) and [Ben Smith](https://benjaminsmith.dev). All credit for the methodology belongs to them. The source is the official documentation at [cube.fyi](https://cube.fyi). The skills are an independent interpretation, not endorsed by the authors.

The layout-primitive ideas CUBE builds on come from [Every Layout](https://every-layout.dev) by Heydon Pickering and Andy Bell.

The skill-writing approach follows the lessons at [aiforui.dev](https://aiforui.dev/learn/how-to-write-good-skills).

## License

[MIT](LICENSE) for the skill text in this repo. The CUBE CSS methodology itself belongs to its authors.
