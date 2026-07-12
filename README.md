# Ruby Markups

Authoring specifications for **CJK `<ruby>` markup** — written to be read by humans
*and* loaded directly as [Claude Code / LLM skills](https://docs.claude.com/en/docs/claude-code/skills).

Getting ruby right in CJK is less about the `<ruby>` element and more about the
rules *around* it — where punctuation sits, how words are chunked, what breaks
the browser's native kinsoku (標點避頭尾) and inter-character (字旁直書) layout.
Those rules are subtle, load-bearing, and rarely written down. This repo writes
them down.

## Specs

| Spec | Covers | Status |
| --- | --- | --- |
| [`zhuyin-ruby-markup/`](./zhuyin-ruby-markup/) | Zhuyin (注音 / MPS) ruby — one-base-per-ruby, punctuation adjacency, nested dual annotation, tone-mark order, polyphone handling | ✅ stable |

Pinyin, furigana (振り仮名), Cantonese Jyutping, and Latin-gloss ruby may follow —
each is a *different* layout problem, so each gets its own spec rather than being
folded into Zhuyin's rules.

## Using a spec as an LLM skill

Each spec folder is a self-contained skill: a `SKILL.md` with YAML frontmatter
plus `examples/`. To install into a Claude Code project:

```sh
# from your project root
git clone https://github.com/hanzipro/ruby-markups /tmp/ruby-markups
cp -R /tmp/ruby-markups/zhuyin-ruby-markup .claude/skills/
```

The frontmatter `description` triggers the skill whenever the model touches
`class="zhuyin"` / `class="mps"` markup, so it applies even to a bare
"add zhuyin to this text" request.

## Using a spec as a human reference

Just read the `SKILL.md` — it is prose with rationale, not a config file. Open
the files in each spec's `examples/` in a browser to see the markup rendered.
(Native inter-character positioning is honoured by Safari; other engines need an
inter-character CSS fallback + a Zhuyin webfont — see below.)

## Reference implementation

These specs are extracted from **[Han.css](https://css.hanzi.pro)**, which ships
the Zhuyin webfont, the inter-character CSS fallback, and the legacy-markup
conversion helper referenced in the Zhuyin spec. Han.css is the canonical
consumer, but every rule here is engine-agnostic — nothing requires Han.css
specifically.

## License

[CC BY 4.0](./LICENSE). Use the rules, ship the markup, quote the prose — just
keep attribution.
