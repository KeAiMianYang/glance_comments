# glance (fork with comment extraction)

Fork of [lpil/glance](https://github.com/lpil/glance) — a Gleam source code parser, in Gleam.

Upstream version: **6.0.0**

## What this fork adds

Upstream glance discards comments during lexing. This fork preserves them by
replacing `glexer.discard_comments` with a `collect_comments` pipeline that
extracts comments from the token stream before parsing.

New types:

- `CommentKind` — `RegularComment` (`//`), `DocComment` (`///`), `ModuleComment` (`////`)
- `Comment` — holds `kind`, `text`, and byte-offset `Span`

The `Module` record gains a `comments: List(Comment)` field. Consecutive
doc/module comments of the same kind are merged into a single `Comment` with
newline-joined text and a span covering the full range.

## Upstream tracking

| Remote   | URL                                                   |
| -------- | ----------------------------------------------------- |
| origin   | `https://github.com/KeAiMianYang/glance_comments.git` |
| upstream | `https://github.com/lpil/glance`                      |

To sync with upstream: `git fetch upstream && git merge upstream/main`, then
resolve any conflicts in `src/glance.gleam` around the comment-extraction code.

## Usage

```gleam
import glance

pub fn main() {
  let assert Ok(parsed) = glance.module("/// A doc comment\npub fn main() { Nil }")
  echo parsed.comments
  // [Comment(DocComment, " A doc comment", Span(0, 17))]
}
```
