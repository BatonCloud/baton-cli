# Translations

**English is canonical.** Every other language follows `README.md`; where a
translation and the English text disagree, the English text is correct and the
translation is the defect to be fixed.

## Current languages

| Language | File | Status |
|---|---|---|
| English | [`README.md`](README.md) | Canonical |
| 简体中文 | [`README.zh-CN.md`](README.zh-CN.md) | Current |

More languages are added as contributors bring them. There is no target list —
a language is supported when someone maintains it, and we would rather have two
accurate translations than eight that quietly rot.

## Adding a language

1. Copy `README.md` to `README.<locale>.md`, using a
   [BCP 47](https://www.rfc-editor.org/info/bcp47) tag: `README.ja.md`,
   `README.pt-BR.md`, `README.de.md`.
2. Translate it. Keep the section order and heading structure identical to the
   English file — that is what makes drift visible later.
3. Update the language switcher line at the top of **every** README, including
   the ones you did not write. The line lists all languages; the current one is
   bold and unlinked.
4. Add a row to the table above.
5. Open one pull request containing exactly that.

## What not to translate

- **Command names, flags, package names, and file paths.** `baton create agent`
  is `baton create agent` in every language.
- **The product category line** — `Baton — Open-Source Agent & Network Runtime
  Framework` — stays in English. It is a name, not a sentence.
- **Status words in the status table** (`Implemented`, `Do not exist`) may be
  translated, but the verb lists next to them may not.

## Keeping a translation honest

When `README.md` changes materially, the translations are stale until someone
updates them. A stale translation that still reads as current is worse than a
missing one, because it makes a claim on the project's behalf that the project
no longer makes.

If you find a translation that has drifted, say so in an issue even if you
cannot fix it. Naming the drift is a contribution.
