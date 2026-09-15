# CLAUDE.md — LFD Textbook

## What this repo is

The online textbook for **"Learning From Data (and) Science" (LFD)**, an MRes module at Warwick Business School taught by Professor Nick Lee. Built with bookdown, published to GitHub Pages from `main` + `docs/`.

The book covers the quantitative half of a nine-lecture module. It is a **supplement to in-person lectures**, not a standalone text — chapters regularly hand back to the slides with lines like "let's return to the slide deck". Preserve those handoffs.

The current revision plan is in `revisions-2026.md`. Read it when working on a task; don't assume its contents.

## House style

These override generic textbook convention wherever the two conflict.

1. **Front-load conceptual decisions.** Explain *why* before the calculation, never mid-worked-example. Cognitive load is the enemy. If a choice has to be made (one- vs two-tailed, whether to standardise, which test), the reasoning goes before the arithmetic.

2. **Numerical differences between book and slides are intentional.** Simulation chapters reseed on every render, so the numbers legitimately differ from the lecture slides. Never "fix" this. Chapter 5 already flags it to students.

3. **Acknowledge scepticism rather than deflecting it.** When students challenge something — bootstrapping, arbitrary thresholds, the nil hypothesis — the honest, self-aware answer builds more credibility than a defensive one. If a step genuinely is a bit redundant, say so.

4. **Decision rules are heuristics, not laws.** The book is consistently critical of `p < 0.05` fetishism, significance stars, and "marginally significant". Do not soften this into conventional hedging. Do not add "however, the 0.05 threshold is widely accepted" style qualifications.

5. **This is not an R course.** Code serves the concept. Never add programming exposition, package tutorials, or explanations of R syntax. Students are learning to interpret quantitative evidence, not to code.

6. **Nick's voice.** First person, informal, willing to be self-deprecating and to use real examples from his own career. He tells stories against himself (telling a 1990s class to "just look for the stars"; his own papers using "marginal significance"). Do not sand this into neutral textbook prose. If a passage reads like it could be from any statistics textbook, it's wrong.

## Build

```r
bookdown::render_book('index.Rmd', 'bookdown::gitbook')
```

**Nothing is done until the book builds.** Chapters run live R against files in `Data/`, so a change that reads correctly can still fail at knit time. Always render before committing.

Preview locally — open `docs/index.html`, or run `servr::httw("docs")` in R for an exact replica of the published site with working search. **Do not deploy.**

## Git

Nick is not experienced with branching workflows. Be explicit about what you are doing and which branch you are on at every point.

- All work happens on **`revisions-2026`**. `main` stays untouched.
- Pages serves from `main` + `docs/`, so the published 2025 book must keep serving until Nick approves a merge.
- Merge to `main` **only** when Nick explicitly says so.
- Commit in meaningful chunks with descriptive messages, so Nick can review and revert selectively. Not one large commit.
- `docs/` contains committed build output. If a merge conflicts in generated HTML, take either side and re-run `render_book`, then commit the fresh output. Never hand-resolve generated files.

## Repo structure

```
index.Rmd              Front matter + Chapter 1
NN-*.Rmd               Chapter files (see map below)
12-references.Rmd      References
_bookdown.yml          book_filename, output_dir: "docs"
_output.yml            gitbook config, TOC, edit link, download formats
style.css, toc.css     Styling
book.bib, packages.bib Bibliography
preamble.tex           LaTeX preamble (pdf_book only)
Data/                  All .xlsx data files
docs/                  Committed build output — served by Pages
```

## Chapter numbering — read this before touching cross-references

**Chapter files are numbered one lower than the chapter they render as**, because `index.Rmd` renders as Chapter 1. Filename numbers exist only to force alphabetical ordering; `_bookdown.yml` has no `rmd_files:` entry and nothing reads them as chapter numbers.

Nick's slides and spoken lecture references use the **rendered** numbers. Do not renumber files. Do not unnumber the `index.Rmd` heading to close the offset — that would shift every rendered number and invalidate two lectures' worth of slide references.

| File | Renders as | Label | Lecture |
|---|---|---|---|
| `index.Rmd` | Ch 1 | — | — |
| `01-intro.Rmd` | Ch 2 | `{#intro}` | 2 |
| `02-Distributions.rmd` | Ch 3 | `{#norm-dist}` | 3 |
| `03-assoc_rel.Rmd` | Ch 4 | `{#assoc-rel}` | 5 |
| `04-estimation.Rmd` | Ch 5 | `{#uncertainty}` | 6, pt 1 |
| `05-bootstrapping.Rmd` | Ch 6 | `{#bootstrap}` | 6, pt 2 |
| `06-t-test.Rmd` | Ch 7 | `{#t-test}` | 6, pt 2 |
| `07-probability.Rmd` | Ch 8 | `{#probability}` | 7, pt 1 |
| `08-statistics.Rmd` | Ch 9 | `{#statistics}` | 7, pt 2 |
| `09-H-Testing.Rmd` | Ch 10 | `{#H-testing}` | 8, pt 1 |
| `10-ANOVA.Rmd` | Ch 11 | `{#ANOVA}` | 8, pt 2 |
| `11-Issues_w_sig.Rmd` | Ch 12 | `{#Issues}` | 8 pt 3 + parts of 9 |

Lectures 1 and 4 have no chapters. This is expected and stated to students in `index.Rmd`.

**Always cross-reference with `\@ref(label)`, never a hardcoded chapter number.** Every chapter carries a label. `09-H-Testing.Rmd` already uses `\@ref(conf)` correctly — follow that pattern.

## Traps

**Do not edit files in `ACTUAL_NOTE_CODE/`.** They are superseded 2023 drafts with hardcoded `D:/Dropbox/R_Files/Data/…` paths, close enough to the live chapters to be mistaken for them. They are scheduled for deletion. If they still exist, the live chapter is always the numbered file in the repo root.

**Chapters share a single R session in the merged build.** Bookdown merges everything before knitting, so a chapter can depend on a library or object an earlier one loaded, and pass cleanly in the merged build while failing standalone — this is exactly how `05-bootstrapping.Rmd`'s missing `library(psych)` surfaced during task 3.3 (it relied on `03-assoc_rel.Rmd` having already loaded it). Do not assume a chapter is self-contained; verify with `rmarkdown::render()` on the single file, not only `bookdown::render_book()` on the whole book.

**When a task's stated assumption doesn't match what the code actually does, stop and flag it rather than resolving it unilaterally** — a claimed seed, a claimed dependency, a claimed library, or anything else `revisions-2026.md` asserts as fact. This happened twice in the 2026 revision (the `assoc_rel` label's underscore bug, and `04-estimation.Rmd` turning out to have no seed despite the worklist assuming one), and both times the right call differed from what looked obvious at first glance.

**`installr` was removed from all five chapters that loaded it** (task 3.1). It's a Windows-only R-updating utility that does nothing for the book and will fail on Linux or in a container — remove it again if it reappears.

**Two data files are named after the Ed Sheeran example** (`SHEERAN_T.xlsx`, `SHEERAN_ANOVA.xlsx`). That example is being replaced. Name new data files after the construct, not the stimulus, so future swaps are prose edits.

**Small sample sizes are deliberate.** The music/anger study uses n=15 per group specifically so that Chapter 12's power calculation shows it was underpowered. Never "improve" a sample size without checking what downstream analysis depends on it.

## Scope

- The **slides are not in this repo.** Changes affecting them (example swaps, terminology) must be flagged to Nick, not actioned.
- Nick is the sole author. Never add a co-author, acknowledgement, or attribution to a chapter.
- Never invent statistical results, cite papers Nick hasn't referenced, or add references not already in the bibliography.
