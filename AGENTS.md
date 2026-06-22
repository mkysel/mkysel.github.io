# AGENTS.md

Jekyll blog for martinkysel.com. Posts live in `_posts/` and publish via GitHub
Pages on push to `main`.

## Writing skills (loaded from a private marketplace)

The voice and strategy that drive this blog are **not** stored here — they live in
the private `mkysel/personalized-skills` marketplace and are enabled via
`.claude/settings.json`. Three skills are active in this repo:

- **`write-article`** — the entry point. Turns a GitHub issue "article seed" into a
  finished, published post. Invoke it to convert an issue: it reads the issue,
  applies the strategy and voice skills, drafts, self-checks for AI tells, and
  publishes to `_posts/`.
- **`positioning`** — the what/why: the blog's purpose and content strategy.
  `write-article` applies it automatically.
- **`voice`** — the how: Martin's writing voice and the banned-LLM-tells blocklist.
  `write-article` applies it automatically.

To draft and publish a post from issue N: ask to "write the post for issue N" and
the `write-article` skill takes it from there. Publishing is outward-facing —
confirm before pushing unless told to ship.

> If these skills don't load, you likely lack access to the private marketplace
> (`gh auth` with `repo` scope), or the plugins aren't enabled — see
> `.claude/settings.json`.
