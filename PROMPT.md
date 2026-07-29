# Cheatsheet Generator Prompt

Paste everything below into any capable AI, then paste your source material
(course syllabus, docs, notes, transcript) underneath it.

---

You are a senior engineer with a decade of production experience, writing a
revision sheet for another senior engineer. Your reader knows how to program.
They do not need concepts explained from scratch — they need the *exact shape of
the thing they'd type*, plus the trap that wastes their afternoon.

I will give you source material on a technical topic. Produce **one
self-contained HTML file**: a dense, filterable, printable cheatsheet.

## Content rules

**Extract, then compress.** Read the source and pull out every distinct concept.
Group them into 8–14 sections that follow a learning order, not the source's
chapter order. Within a section, one `.card` per concept.

**A card is the minimum needed to recall the thing.** Usually that's a code block
in the exact syntax the reader would type, plus one line of framing. Prose is the
fallback, not the default. If a card has three sentences and no code, ask whether
it's really a card.

**Prefer the real thing over the toy example.** Show `actions/checkout@v4`, not
`some-action@version`. Use realistic names, realistic values, realistic paths.

**Write the gotchas.** For each major concept, ask: what silently fails here?
What did I get wrong the first three times? Those belong in `.note` blocks and are
often the most valuable content on the page. Draw on your own experience — the
source material won't contain most of them.

**Correct the source.** If it teaches deprecated or outdated syntax, write the
current syntax and add a note saying what changed. Never reproduce something that
no longer works just because the source said it.

**Use comparison tables** wherever two things are confused for each other
(`x` vs `y`, "which one do I want?"). These are high-value and easy to scan.

**End with a "Last-minute recall" section**: a card of 8–12 one-line reflexes, a
card of syntax traps specific to the language/format, and any limits, quotas or
costs worth knowing.

**Target 50–90 cards.** Density is the point. Someone should be able to scan the
whole page in five minutes and land on the one card they needed.

## Design system — use these values exactly

```css
--paper:#E7E9E1;   --card:#FBFBF7;   --ink:#171B17;    --ink-2:#5C6359;
--rule:#CBCFC1;    --volt:#4B31C8;   --volt-soft:#EAE6FA;
--amber:#A8660F;   --amber-soft:#F6EBD9;
--moss:#2C6740;    --moss-soft:#E1EDE2;
--crimson:#A72B29;
--code-bg:#141914; --code-fg:#DFE5D8; --code-key:#9FC4F0;
--code-str:#C7DFA0; --code-com:#7C8878;
```

Fonts, loaded from Google Fonts with system fallbacks:

```html
<link href="https://fonts.googleapis.com/css2?family=Archivo:wght@500;700;800&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
```

`Archivo` for display and body, `JetBrains Mono` for code, labels and eyebrows.
Body background is `--paper` with a 56px blueprint grid drawn via two
`linear-gradient` layers in `--rule`.

## Required structure

1. **Masthead** — a `← All cheatsheets` link to `../index.html`, a mono eyebrow
   line, an oversized uppercase `h1` (clamp to ~92px, weight 800, letter-spacing
   -.035em) with one word wrapped in `<em>` colored `--volt`, and a one-sentence
   subtitle.

2. **Hero** — a two-column grid. Left: the single most characteristic artifact of
   the subject (a canonical config file, a representative command sequence, a core
   API call) as one annotated code block, with `← 1`, `← 2` markers in comment
   color. Right: an aside listing what each numbered part does. This is the "read
   it once and you've got the model" moment. Make it count.

3. **Sticky filter bar** — a search input plus a live "N of M cards" counter.
   Typing hides non-matching cards and collapses empty sections. `/` focuses it,
   `Esc` clears it. Show a "no match" message when nothing hits.

4. **Two-column body** — a sticky left rail of section links styled as connected
   nodes (a vertical hairline with a dot per link), and the main content. On
   screens under 1000px the rail becomes a horizontal scrolling row of chips.

5. **Sections** — each with an `h2` in uppercase over a 2px rule, plus a mono
   subtitle pushed right. Cards inside a
   `grid-template-columns: repeat(auto-fill, minmax(330px,1fr))` with
   `align-items: start`. Use `.wide` (span 2) for the two or three most important
   cards per sheet.

6. **Footer** — a 2px top rule and two mono lines.

## Component markup

```html
<div class="card">
  <h3>Title in caps</h3>          <!-- gets a --volt square marker via ::before -->
  <p>One line of framing.</p>
  <pre>…</pre>
  <div class="note gotcha"><b>Gotcha</b>The thing that silently fails.</div>
</div>
```

Note variants — use them for meaning, not decoration:

| Class | Border | Use for |
|---|---|---|
| `.note.tip` | `--moss` | the better way, the practice to adopt |
| `.note.gotcha` | `--amber` | surprising behaviour, easy mistake |
| `.note.danger` | `--crimson` | security risk, data loss, irreversible |

Key/value tables use `td:first-child` in mono, nowrap, `width:1%`, with hairline
row borders and none on the last row.

Code blocks are dark (`--code-bg`) on the light page. Inside `<pre>`, apply
syntax color with plain tags — no highlighting library:

- `<b>` → keys / keywords, colored `--code-key`, weight 500
- `<i>` → values / strings, colored `--code-str`, `font-style: normal`
- `<s>` → comments, colored `--code-com`, `text-decoration: none`

Escape `<`, `>` and `&` as entities inside `<pre>`.

Inline `<code>` is mono on `--volt-soft` in `--volt`, `white-space: nowrap` by
default but `normal` when inside a `<p>` or `<li>`.

## Constraints

- **One file.** All CSS in a `<style>` block, all JS in one `<script>` at the end.
  No frameworks, no CDN scripts, no build step. The only external request is the
  font link, and the page must look intentional if it fails.
- **No `localStorage` or `sessionStorage`.** Keep state in JS variables.
- **Print stylesheet.** White background, hide the filter bar, rail and hero
  aside, two-column card grid, light-background code blocks with dark text,
  `break-inside: avoid` on cards and `<pre>`.
- **Responsive** to 360px. Cards stack, `.wide` becomes single-column, rail turns
  into chips.
- **Accessible.** Visible `:focus-visible` outlines, a labelled search input,
  real heading hierarchy, `prefers-reduced-motion` respected.
- **Restraint.** No gradients, no drop shadows, no emoji, no animated
  decorations. The density of the content is the aesthetic. Everything on the page
  should be information.

## Also give me

The index entry, ready to paste into the shelf:

```html
<a class="sheet" href="sheets/FILE-NAME.html">
  <span class="idx">NN</span>
  <span class="meat">
    <h2>SHEET TITLE</h2>
    <p>One or two sentences naming the topics inside — these words are what the
       filter searches, so use the keywords someone would actually type.</p>
    <span class="tags"><span>TAG</span><span>TAG</span></span>
  </span>
  <span class="side">~NN cards<span class="arrow">&rarr;</span></span>
</a>
```

And a short note on anything you corrected, added from experience, or judged
important enough to include even though the source didn't mention it.

## Before you finish, check

- Would a senior engineer learn something from at least five of these cards?
- Is every code block something you'd actually type, or is some of it pseudo-code?
- Does any card exist only because the source had a chapter on it?
- Are the `.note` colors used by meaning, or scattered for visual variety?
- Does the filter still work if a section ends up empty?
- Does it print to a clean PDF?

---

**Now here is the source material:**
