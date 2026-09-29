# CLAUDE.md — LFD Textbook

## What this repo is

The online textbook for **"Learning From Data (and) Science" (LFD)**, an MRes module at Warwick Business School taught by Professor Nick Lee. Built with bookdown, published to GitHub Pages from `main` + `docs/`.

The book covers the quantitative half of a nine-lecture module. It is a **supplement to in-person lectures**, not a standalone text — chapters regularly hand back to the slides with lines like "let's return to the slide deck". Preserve those handoffs.

The current revision plan is in `revisions-2026.md`. Read it when working on a task; don't assume its contents.

## House style

These override generic textbook convention wherever the two conflict.

1. **Front-load conceptual decisions.** Explain *why* before the calculation, never mid-worked-example. Cognitive load is the enemy. If a choice has to be made (one- vs two-tailed, whether to standardise, which test), the reasoning goes before the arithmetic.

2. **Numerical differences between book and slides are intentional.** Simulation chapters reseed on every render, so the numbers legitimately differ from the lecture slides. Never "fix" this. `05-estimation.Rmd` (Ch 6) and the "How this book works" section in `index.Rmd` already flag it to students. Refer to files by name, not chapter number, so the reference can't drift after renumbering.

3. **Acknowledge scepticism rather than deflecting it.** When students challenge something — bootstrapping, arbitrary thresholds, the nil hypothesis — the honest, self-aware answer builds more credibility than a defensive one. If a step genuinely is a bit redundant, say so.

4. **Decision rules are heuristics, not laws.** The book is consistently critical of `p < 0.05` fetishism, significance stars, and "marginally significant". Do not soften this into conventional hedging. Do not add "however, the 0.05 threshold is widely accepted" style qualifications.

5. **This is not an R course.** Code serves the concept. Never add programming exposition, package tutorials, or explanations of R syntax. Students are learning to interpret quantitative evidence, not to code.

6. **Nick's voice.** First person, informal, willing to be self-deprecating and to use real examples from his own career. He tells stories against himself (telling a 1990s class to "just look for the stars"; his own papers using "marginal significance"). Do not sand this into neutral textbook prose. If a passage reads like it could be from any statistics textbook, it's wrong.

7. **Code is never shown.** Analyses run live, but the code is kept out of view. Before adding a chunk, check how the chapter hides code (a global knitr option or per-chunk `echo=FALSE`) and follow that pattern, then confirm that no code is visible in the rendered output. Never describe the book as offering code to run, read or modify.

8. **General management framing.** Students come from every management discipline, and marketing students are a minority. Use general examples, not marketing-specific ones.

9. **Don't create research myths.** Where a common concern is conditional (response rates, outliers, significance tests for bias), say what it depends on rather than presenting it as automatically bad. Myths in this field are usually oversimplified caveats.

10. **Never assume a later chapter has been read.** Pointing ahead is fine ("as you'll see in Chapter \@ref(H-testing)"). Writing as if the reader already knows later material ("you already know from Chapter 11") is not. References back to earlier chapters are fine.

11. **Personal stories come only from Nick.** Anecdotes and first-person claims must come from his lectures or from what he has said directly. Never invent them to fit the voice. Mentions of family need his explicit approval.

12. **Real papers are cited; critique by name is reserved for verifiable case studies.** Where a passage illustrates bad practice in general, the example (a reporting snippet, a set of figures) is invented and presented as illustrative. Real people and papers may be named only in agreed case studies where the account is verifiable, fair and factual, with every claim traceable to a cited primary source (for example, the power pose section `{#power-pose}` in `12-Issues_w_sig.Rmd`). Such cases describe what happened and what the people involved have said; they do not speculate about motives.

## Reading-the-literature sections

Chapters may end with a reader section (`{#<label>-reading}`) that helps students *read* papers, not run analyses. Not every chapter needs one; where a chapter has little to say to a reader, it can be folded into a neighbour's section instead of padded out. A reader section may cover methods well beyond the book (IV, control functions, fixed effects, DiD, Heckman and so on), but only at reading level: what the number claims, which core concept from the chapter it rests on, what assumption it needs, and what to question. No code, and no how-to.

Template:
1. Opening frame
2. Where you'll meet this (in heavy chapters, organised into method families, each with one characteristic assumption)
3. The connection back to the chapter's core concept
4. Questions worth asking: judgement questions, never a tick-box checklist (the book argues against ritual statistics, so the checklist must not become a new ritual)
5. One red flag
6. If you want to read more: one or two sources chosen for this audience
7. Pointer to the reader-guide appendix (commented out until the appendix exists)

Budget is roughly 600–900 words, and up to about 1,500 for heavy chapters. Causation is the accepted longer exception.

**Conceptual explanation belongs in the chapter body.** Reader sections focus on the methods readers will meet in papers. If a reader section needs a conceptual explanation to work, put that explanation in the body and link to it.

The reader-guide appendix is planned but not yet built. When it is, use `# (APPENDIX) Appendix {-}` so that it is lettered and no chapters renumber, and organise it in the order a paper is read.

Existing models: Ch 2 `{#intro-reading}`, Ch 4 `{#causation-reading}`, Ch 13 `{#issues-reading}`.

## Simulation and figure conventions

- **Numbers in prose come from the code via inline R**, never hard-coded, so the text can't drift from the figures. Put any setup chunk *before* the first inline reference to its results.
- **Slow simulations** use `cache=TRUE`, and are kept fast enough not to burden every render. The p-hacking simulation in `12-Issues_w_sig.Rmd` is the model: a hidden setup chunk plus a separate plotting chunk.
- **New chunks share the book's single R session** (see Traps). Check new object names against later uses in the same chapter and in later chapters.
- **Test figures in isolation** before they go in a chapter, and check them at narrow widths.
- **Figure and caption styling is global,** in `style.css`. Don't style individual figures.

## Verification

- **Verify technical claims as well as citations.** Compressed one-line descriptions of methods are where errors creep in.
- **Check every new reference against a primary source** before it is committed. Flag anything unverified to Nick at the stop point.
- **Web sources** carry an access date.

## Build

```r
bookdown::render_book('index.Rmd', 'bookdown::gitbook')
```

**Nothing is done until the book builds.** Chapters run live R against files in `Data/`, so a change that reads correctly can still fail at knit time. Always render before committing.

Preview locally — open `docs/index.html`, or run `servr::httw("docs")` in R for an exact replica of the published site with working search. **Do not deploy.**

## Git

Nick is not experienced with branching workflows. Be explicit about what you are doing and which branch you are on at every point.

- Work happens one branch per part: **`revisions-2026-partN`** (e.g. `revisions-2026-part3`), branched from `main`. `main` stays untouched.
- Pages serves from `main` + `docs/`, so the published 2025 book must keep serving until Nick approves a merge.
- Merge to `main` **only** when Nick explicitly says so.
- Tasks arrive as handover files in the repo root. These files stay **uncommitted** and are deleted once the work is done. Where the prompt Nick gives you differs from the handover file, follow the prompt.
- Where prose was drafted without access to the chapter file, **stop for a content check before inserting anything**: report placement, overlaps and cross-reference problems, and wait for Nick's decisions.
- **Report before committing**: placement, render results, cross-references, citations, and anything that didn't match the handover. Wait for Nick's go-ahead.
- One commit per handover, with a descriptive message, so Nick can review and revert selectively.
- **Merging a part branch.**
  1. Do a clean full render and commit the fresh `docs/` before the merge.
  2. Nick merges on GitHub using "Create a merge commit", not squash, so the per-handover commits stay revertible.
  3. After the merge, pull `main`, tag the result `v2026-partN`, and push the tag.
  4. Delete the branch with `git branch -d` and `git push origin --delete`. Use `-d`, never `-D`, so git refuses if anything is unmerged.
- `docs/` contains committed build output. If a merge conflicts in generated HTML, take either side and re-run `render_book`, then commit the fresh output. Never hand-resolve generated files.

## Repo structure

```
index.Rmd              Front matter + Chapter 1
NN-*.Rmd               Chapter files (see map below)
13-references.Rmd      References
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

Nick's slides and spoken lecture references use the **rendered** numbers. Do not renumber files further without checking with Nick first, and do not unnumber the `index.Rmd` heading to close the offset — that would shift every rendered number again. This table was last revised when `03-causation.Rmd` was inserted, deliberately shifting every rendered number from Ch 5 onward and invalidating slide references from Lecture 5 on; Nick accepted that cascade and is updating slide-deck references separately, week by week.

| File | Renders as | Label | Lecture |
|---|---|---|---|
| `index.Rmd` | Ch 1 | — | — |
| `01-intro.Rmd` | Ch 2 | `{#intro}` | 2 |
| `02-Distributions.rmd` | Ch 3 | `{#norm-dist}` | 3 |
| `03-causation.Rmd` | Ch 4 | `{#causation}` | 4 |
| `04-assoc_rel.Rmd` | Ch 5 | `{#assoc-rel}` | 5 |
| `05-estimation.Rmd` | Ch 6 | `{#uncertainty}` | 6, pt 1 |
| `06-bootstrapping.Rmd` | Ch 7 | `{#bootstrap}` | 6, pt 2 |
| `07-t-test.Rmd` | Ch 8 | `{#t-test}` | 6, pt 2 |
| `08-probability.Rmd` | Ch 9 | `{#probability}` | 7, pt 1 |
| `09-statistics.Rmd` | Ch 10 | `{#statistics}` | 7, pt 2 |
| `10-H-Testing.Rmd` | Ch 11 | `{#H-testing}` | 8, pt 1 |
| `11-ANOVA.Rmd` | Ch 12 | `{#ANOVA}` | 8, pt 2 |
| `12-Issues_w_sig.Rmd` | Ch 13 | `{#Issues}` | 8 pt 3 + parts of 9 |

Lecture 1 has no chapter. This is expected — `index.Rmd` tells students that chapter numbers don't map onto lecture numbers, since not every lecture has quantitative content. Lecture 4 ("What is a Cause and How do you Know?") used to have no chapter either, for the same reason, until `03-causation.Rmd` was added to cover it.

**Always cross-reference with `\@ref(label)`, never a hardcoded chapter number.** Every chapter carries a label. `10-H-Testing.Rmd` already uses `\@ref(conf)` correctly (the `{#conf}` label itself lives in `09-statistics.Rmd`) — follow that pattern.

## Traps

**Do not edit files in `ACTUAL_NOTE_CODE/`.** They are superseded 2023 drafts with hardcoded `D:/Dropbox/R_Files/Data/…` paths, close enough to the live chapters to be mistaken for them. They are scheduled for deletion. If they still exist, the live chapter is always the numbered file in the repo root.

**Chapters share a single R session in the merged build.** Bookdown merges everything before knitting, so a chapter can depend on a library or object an earlier one loaded, and pass cleanly in the merged build while failing standalone — this is exactly how `06-bootstrapping.Rmd`'s missing `library(psych)` surfaced during task 3.3 (it relied on `04-assoc_rel.Rmd`, then numbered `03-assoc_rel.Rmd`, having already loaded it). Do not assume a chapter is self-contained; verify with `rmarkdown::render()` on the single file, not only `bookdown::render_book()` on the whole book.

**When a task's stated assumption doesn't match what the code actually does, stop and flag it rather than resolving it unilaterally** — a claimed seed, a claimed dependency, a claimed library, or anything else `revisions-2026.md` asserts as fact. This has happened three times in the 2026 revision, and each time the right call differed from what looked obvious at first glance:
- the `assoc_rel` label's underscore bug;
- `04-estimation.Rmd` turning out to have no seed, despite the worklist assuming one;
- spec §7.2 describing p-hacking, forking-paths and pre-registration content in the Issues chapter that didn't exist.

**`installr` was removed from all five chapters that loaded it** (task 3.1). It's a Windows-only R-updating utility that does nothing for the book and will fail on Linux or in a container — remove it again if it reappears.

**Two data files are named after the Ed Sheeran example** (`SHEERAN_T.xlsx`, `SHEERAN_ANOVA.xlsx`). That example is being replaced. Name new data files after the construct, not the stimulus, so future swaps are prose edits.

**Small sample sizes are deliberate.** The music/anger study uses n=15 per group specifically so that Chapter 13's power calculation shows it was underpowered. Never "improve" a sample size without checking what downstream analysis depends on it.

**No live embedded interactive widgets.** Task 2.8 removed an embedded Shiny app (`knitr::include_app()`) because it only ever wrote an iframe at render time — a successful book render said nothing about whether the app actually loaded for a reader, and it was already erroring in the browser by the time this was caught. Replaced with a self-contained static ggplot of the underlying formula, annotated with the specific values the app was used to illustrate. Follow that pattern for anything that might look like it wants a live widget — a static, annotated figure — rather than introducing a new embed dependency.

## Scope

- The **slides are not in this repo.** Changes affecting them (example swaps, terminology) must be flagged to Nick, not actioned.
- Nick is the sole author. Never add a co-author, acknowledgement, or attribution to a chapter.
- Never invent statistical results.
- New references may be added only where Nick has named or approved the source. Each must be verified against its primary source (see Verification) before committing.
