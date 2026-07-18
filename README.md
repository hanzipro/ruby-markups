# Ruby Markups

Authoring rules for **CJK `<ruby>` markup** — written to be read by humans
*and* loaded directly as a [Claude Code / LLM skill](https://docs.claude.com/en/docs/claude-code/skills).

Getting ruby right in CJK is less about the `<ruby>` element and more about the
rules *around* it — where punctuation sits, how words are chunked, what breaks
the browser's native kinsoku (標點避頭尾) and annotation layout. Those rules
are subtle, load-bearing, and rarely written down. This repo writes them down.

## One spec, every annotation type

[`SKILL.md`](./SKILL.md) covers all CJK ruby annotation in two tiers:

- **Universal rules** — punctuation adjacency & kinsoku, nested multi-layer
  annotation, `rb`/`rtc` interop handling, reading-standard & polyphone
  confirmation. These bind any annotation language.
- **Per-type sections** — Zhuyin (注音 / MPS) is the deepest, because it
  alone can render inter-character (字間注), making it the most constrained:
  one-base-per-ruby, tone-mark order, MOE reading default. Hanyu Pinyin,
  furigana (振り仮名), and Jyutping (粵拼) sections are skeletons that grow
  as needed.

Layout implementation (fonts, CSS fallbacks, engine behavior) is out of
scope — the spec governs how the markup is *written*.

## Using the spec as an LLM skill

The repo root **is** the skill — install by cloning straight into your
skills directory:

```sh
git clone https://github.com/hanzipro/ruby-markups .claude/skills/ruby-markup
```

`git pull` updates it in place. Prefer a copy without `.git`?

```sh
npx degit hanzipro/ruby-markups .claude/skills/ruby-markup
```

The frontmatter `description` triggers the skill whenever the model touches
ruby-annotated CJK — even a bare "add zhuyin to this text" request.

## Using the spec as a human reference

Just read [`SKILL.md`](./SKILL.md) — it is prose with rationale, not a config
file. Open the files in [`examples/`](./examples/) in a browser to see the
markup rendered. (Native inter-character positioning is honoured by Safari;
other engines need an inter-character CSS fallback + a Zhuyin webfont — see
below.)

## Reference implementation

These rules are extracted from **[Han.css](https://hanzi.pro)**, which
ships the Zhuyin webfont, the inter-character CSS fallback, and the
`transpileRuby` down-leveler referenced in the spec. Han.css is the canonical
consumer, but every rule here is engine-agnostic — nothing requires Han.css
specifically.

## License

[CC BY 4.0](./LICENSE). Use the rules, ship the markup, quote the prose — just
keep attribution.
