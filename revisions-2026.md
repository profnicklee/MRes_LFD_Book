# LFD Textbook — 2026 Revision Worklist

This is the execution plan for the 2026 revision of the LFD bookdown textbook. It is written to be actioned by Claude Code without further explanation of the pedagogy behind each item.

Read `CLAUDE.md` in the repo root before starting. It carries the house style, which takes precedence over generic textbook convention anywhere the two conflict.

## How this document is structured

**Part One — Priorities 0 to 5.** The committed 2026 revision. Everything here is agreed and scoped. Work these in order.

**Part Two — The confounding demonstration.** A single high-value addition, to be started only if Part One completes with time in hand. Fully specified so it can be picked up without further discussion.

**Part Three — Deferred.** Two further additions, not to be started until Parts One and Two are complete. Recorded now mainly so the placement reasoning isn't lost.

Parts Two and Three both add content for lectures the book currently doesn't cover. Neither adds a new chapter file — see the note on renumbering in Part Two.

## Working rules

- All work happens on the `revisions-2026` branch. `main` stays untouched — GitHub Pages serves from `main` + `docs/`, so the 2025 book must keep publishing until Nick approves the merge.
- Before any work: `git tag v2025-final && git push origin v2025-final`.
- Nothing is done until the book builds: `bookdown::render_book('index.Rmd', 'bookdown::gitbook')`. Chapters run live R against files in `Data/`, so a change that reads correctly can still fail at knit time.
- Preview locally — open `docs/index.html` or run `servr::httw("docs")`. Do not deploy.
- Commit in meaningful chunks with descriptive messages, so Nick can review and revert selectively.
- `docs/` contains committed build output. If a merge conflicts in generated HTML, take either side and re-run `render_book`, then commit the fresh output. Never hand-resolve generated files.
- Numerical differences between the book and the lecture slides are intentional. Simulation chapters reseed on every render. Never "fix" these.

## File-to-chapter map

Chapter files are numbered one lower than the chapter they render as, because `index.Rmd` renders as Chapter 1. Filename numbers exist only to force alphabetical ordering — `_bookdown.yml` has no `rmd_files:` entry. Nothing reads them as chapter numbers.

Nick's slides and spoken lecture references use the **rendered** numbers. Do not renumber files, and do not unnumber the `index.Rmd` heading to close the offset — that would shift every rendered number and invalidate two lectures' worth of slide references.

| File | Renders as | Title | Lecture |
|---|---|---|---|
| `index.Rmd` | Ch 1 | Introduction and Orientation | — |
| `01-intro.Rmd` | Ch 2 | Describing the World With Data | Lecture 2 |
| `02-Distributions.rmd` | Ch 3 | The Normal Distribution | Lecture 3 |
| `03-assoc_rel.Rmd` | Ch 4 | Associations and Relationships | Lecture 5 |
| `04-estimation.Rmd` | Ch 5 | Beginning to Understand Uncertainty | Lecture 6, pt 1 |
| `05-bootstrapping.Rmd` | Ch 6 | Introduction to Bootstrapping | Lecture 6, pt 2 |
| `06-t-test.Rmd` | Ch 7 | T-Tests for Means | Lecture 6, pt 2 |
| `07-probability.Rmd` | Ch 8 | Introduction to Probability | Lecture 7, pt 1 |
| `08-statistics.Rmd` | Ch 9 | Introduction to Statistics | Lecture 7, pt 2 |
| `09-H-Testing.Rmd` | Ch 10 | Classical Statistical Hypothesis Testing | Lecture 8, pt 1 |
| `10-ANOVA.Rmd` | Ch 11 | ANOVA | Lecture 8, pt 2 |
| `11-Issues_w_sig.Rmd` | Ch 12 | Issues with Significance Testing | Lecture 8 pt 3 + parts of Lecture 9 |
| `12-references.Rmd` | References | — | — |

Lectures 1 and 4 have no chapters. This is expected and is stated to students in `index.Rmd`.

---

# PART ONE — COMMITTED 2026 REVISION

---

# Priority 0 — Clear the decks

Do this first. It costs little and removes a whole class of error from everything after it.

## 0.1 Remove legacy drafts

`ACTUAL_NOTE_CODE/` contains at least three superseded drafts with hardcoded `D:/Dropbox/R_Files/Data/…` paths:

- `LWD2023_06_Estimates.Rmd` — 2023 draft of the Ch 7 material
- `LWD2023_07_Statistics.Rmd` — 2023 draft of the Ch 9 material
- `Lesson_3_FINAL.rmd` — 2023 draft of the Ch 3 material

List the directory before deleting in case there are more. These are close enough to the live chapters to be mistaken for them during a search-and-edit pass, which is the actual risk. Delete the directory (the content is recoverable from the `v2025-final` tag).

## 0.2 Remove bookdown-demo scaffolding

- `bookdown-demo.tex` — stale build output, carries Yihui Xie as author
- `original example chapers/` — demo chapters, and the folder name is misspelled
- `_publish.R` — demo publishing script, still references `bookdown-demo`
- `DESCRIPTION` — reads `Title: Does not matter.`
- `Dockerfile`, `_build.sh` — demo boilerplate

Confirm none are referenced by CI or any GitHub Action before removing. `_build.sh` renders pdf and epub — see task 2.7, which may change the decision on it.

## 0.3 Check `.gitignore` against `output_dir`

`_bookdown.yml` sets `output_dir: "docs"` and Pages serves from `docs/`, so that directory must stay committed. Verify `.gitignore` does not exclude it.

---

# Priority 1 — Content gaps

These are the two places where students who were confused in a lecture have nowhere to go. This is the work that actually improves the module.

## 1.1 Write the missing hypothesis-test walkthrough — `09-H-Testing.Rmd` (Ch 10)

**The problem.** The worked salary example is the spine of Lecture 8. It is where the one- vs two-tailed decision gets explained, and where Nick slows down specifically because students find it hard. It does not exist in the textbook. Chapter 9 uses the salary numbers only for the confidence-interval simulation; Chapter 10 then opens directly on chi-square. One- vs two-tailed appears in the whole book in a single subordinate clause inside the passage on significance stars.

The result is a structural hole: the book demonstrates confidence intervals, then demonstrates tests of correlations and regressions, but never once demonstrates a z-test against a null hypothesis — the thing everything else is built on.

**The task.** Add a new first section to Chapter 10, before the Premier League chi-square section, working the salary example end to end. Source material is the LFD8 transcript, which contains Nick's full spoken version.

Sequence:

1. The research question and why a null is needed at all — wanting to claim WBS PhD graduates earn more, and needing a comparison figure to say it against. The AMA doctoral survey figure of US$145,000 is the null. The observed sample is n=42, mean $170,000, SD 46617.4.
2. **The one- vs two-tailed decision, taken here, before any calculation.** This placement is the point of the task. In the lecture it lands mid-calculation and adds load exactly when students are already loaded. Frame it as a decision about the hypothesis, not about the arithmetic: a directional hypothesis is a stronger claim, and you have to commit to it in advance. Include Nick's own admission that he was privately expecting "bigger" but did not encode it, and so has to run two-tailed — it is a concrete illustration of why the decision comes first.
3. The z calculation and its interpretation: z = 3.5, standardised so the null sits at zero.
4. Converting z to p, and what the resulting p (~0.00047) does and does not mean.
5. The decision, framed as a judgement rather than a rule. Nick's line that he could legitimately conclude either way, and that cutoffs exist because people like rules, belongs here. This is house style rule 4 and should not be softened.
6. A short decision-matrix summary: directional vs non-directional hypothesis, what each implies for the test, and the relationship between the two p-values. Keep it to a small table.

**Acceptance criteria.** A student who missed Lecture 8 can follow the argument from research question to conclusion. The tailedness decision appears before any arithmetic. The chapter's existing sections follow unchanged.

**Cross-reference.** Link back to the confidence interval section with `\@ref(conf)`, which already exists and is already used correctly elsewhere in this file.

## 1.2 Justify the bootstrapped t — `05-bootstrapping.Rmd` and `06-t-test.Rmd` (Ch 6, 7)

**The problem.** Chapter 7 bootstraps a t statistic and never says why. Students ask why the raw mean difference is not used instead, which is a good question that the chapter does not answer.

The likely trigger is an inconsistency in the chapter itself rather than the t: the independent-samples section bootstraps the **t statistic**, and the paired section bootstraps the **mean of the difference**. A reader going straight through sees the same question answered two different ways within a page, with no comment.

**The task.** Three additions. Do not restructure the chapter.

1. Before the first bootstrapped t in Ch 7, add the standardisation logic. The framing Nick arrived at in LFD6: the t is a standardisation of the mean difference, in the same sense as the standardised versus unstandardised estimates from the regression material. An unstandardised mean difference is interpretable in the units of the original scale but depends entirely on that scale; standardising expresses it relative to sample size and variability.
2. Acknowledge the objection rather than deflecting it. Nick's position in the lecture was that the step does feel unnecessary in a purely bootstrap context, since the conclusion — does the interval contain zero — is the same either way. Say so. House style rule 3.
3. Add a sentence at the paired-sample section noting explicitly that it bootstraps the mean difference rather than the t, and that this is the same logic reached by a shorter route.

**Acceptance criteria.** No new statistical machinery is introduced, no material moves, and the chapter still leads with the demonstration rather than the theory.

---

# Priority 2 — Visible defects

Individually trivial. Collectively the difference between a book that looks maintained and one that looks hacked together from a template.

## 2.1 Fix the broken cross-references

All of these hardcode a chapter number that is wrong. Replace each with a `\@ref()` reference to the target chapter's existing label so it resolves to the live number and survives any future reordering.

| File | Current text | Should point to | Label |
|---|---|---|---|
| `08-statistics.Rmd` | "the article I referenced in Chapter 7" | Ch 8 | `probability` |
| `09-H-Testing.Rmd` | "just as we did in Chapter 7" | Ch 8 | `probability` |
| `09-H-Testing.Rmd` | "As we saw in Chapter 7" | Ch 8 | `probability` |
| `11-Issues_w_sig.Rmd` | "the football goals rate-of-change example from Chapter 8" | Ch 9 | `statistics` |
| `11-Issues_w_sig.Rmd` | "the z-test table from the end of Chapter 8" | Ch 9 | `statistics` |
| `05-bootstrapping.Rmd` | "the same analysis as in Chapter 2" | Ch 4 | `assoc_rel` |

Also check `03-assoc_rel.Rmd`'s "I touched on them earlier, in Chapter 2" — this refers to data transformations and may be correct as written, since `01-intro.Rmd` renders as Chapter 2. Verify against the content before changing.

Every chapter already carries a label (`{#intro}`, `{#norm_dist}`, `{#assoc_rel}`, `{#uncertainty}`, `{#bootstrap}`, `{#t-test}`, `{#probability}`, `{#statistics}`, `{#H-testing}`, `{#ANOVA}`, `{#Issues}`). The mechanism is already in use — `09-H-Testing.Rmd` uses `\@ref(conf)` correctly.

## 2.2 Fix the broken heading in `05-bootstrapping.Rmd`

`###Bootstrapping the Original Sample` has no space after the hashes, so pandoc renders it as literal text rather than a heading and it is missing from the table of contents. Add the space.

## 2.3 Fix `_output.yml`

- TOC header reads `WBS DBA Quantitative Methods` — wrong module. Should be the LFD module title.
- The same line is malformed HTML: `<li><a href="./">WBS DBA Quantitative Methods</li>` is missing the closing `</a>`.
- `edit:` points at `https://github.com/rstudio/bookdown-demo/edit/master/%s`, so "Edit this page" on the live site links to Yihui Xie's repo. Point it at Nick's repo, and check whether the default branch is `master` or `main`.

## 2.4 Fix `index.Rmd` front matter

- `github-repo: rstudio/bookdown-demo` — wrong repo.
- Description contains `WBS MrRes` (typo) and credits "the Bookdown Demo written by Hui". Xie's given name is Yihui; the credit is wrong regardless and should simply go.
- Description ends with "The output format for this example is bookdown::gitbook" — leftover template text.

## 2.5 De-hardcode the year in `index.Rmd`

"The current year is 2025, and the book is currently *in progress for 2025.*" goes stale the moment the module starts. Either update to 2026 or make it dynamic with an inline R expression. Flag the choice to Nick rather than deciding.

## 2.6 Refresh `12-references.Rmd`

States that sources "will be compiled here" in future and that links were "all checked as of January 2024". Either act on the promise or reword it, and re-check the URLs.

## 2.7 Resolve the download buttons

`_output.yml` sets `download: ["pdf", "epub"]`, so the gitbook UI renders PDF and EPUB download buttons. If only the gitbook format is ever built, those links 404 on the live site.

Check the published site first. Then either build all three formats (note `pdf_book` requires `preamble.tex` and XeLaTeX, which is a meaningful added build dependency) or remove the `download:` line. Removing it is the lower-risk option. This decision determines whether `_build.sh` is worth keeping in task 0.2.

---

# Priority 3 — Build safety

## 3.1 Remove `installr`

Loaded in `07-probability.Rmd`, `08-statistics.Rmd`, `09-H-Testing.Rmd`, `10-ANOVA.Rmd` and `11-Issues_w_sig.Rmd`. It is a Windows-only utility for updating R installations and does nothing for the book. It will fail on Linux or in a container. Remove it everywhere and confirm the build still passes.

## 3.2 Remove other unused library calls

Verify against actual usage before removing anything.

- `gganimate`, `gifski`, `png` — loaded in several chapters, but the only animation code is commented out in `07-probability.Rmd`.
- `combinat` and `gtools` are both loaded, but `combinations()` comes from `gtools`.
- `08-statistics.Rmd` loads `TeachingDemos` and then redefines `ci.examp` by hand in the chapter anyway.

## 3.3 Break the cross-chapter object dependency

`05-bootstrapping.Rmd` opens with `median(sub.50$smoking)` and loads no data. `sub.50`, `sub.10` and `sub.200` are created in `04-estimation.Rmd`. This builds because bookdown merges all chapters into a single session, but Chapter 6 cannot be knitted on its own, and it undercuts the per-chapter code archive `index.Rmd` promises students.

Make Ch 6 self-sufficient: reload `Data/heart.data.xlsx` and recreate the subsamples at the top of the file. Keep the seed so the numbers stay consistent with Ch 5.

Check the other chapters for the same pattern while in there.

---

# Priority 4 — Invisible tidying

All `echo=FALSE`. No student sees any of it. Do when convenient.

## 4.1 Dead code and stale comments

- `11-Issues_w_sig.Rmd`: the `#L <- -1.96` lines, commented-out waffle code, and the code comment reading "i was rushing to solve the waffle problem that was dumped on me".
- `03-assoc_rel.Rmd`: commented-out `Assoc2` block that is later uncommented and repeated verbatim.
- `08-statistics.Rmd`: commented-out `yearLabels` and `scale_x_continuous` blocks carried over from Spiegelhalter's original.

## 4.2 Re-check the `waffle` package

`11-Issues_w_sig.Rmd` notes waffle was removed from CRAN and replaced with hand-rolled base R. The file already says to reinstate the elegant version if it returns. Check whether it has.

## 4.3 Consistency pass

- `02-Distributions.rmd` is the only chapter with a lowercase `.rmd` extension. Renaming is safe but touches git history — check it does not break the build ordering.
- Library loading conventions vary between chapters. Standardise.
- `library (infer)` has a stray space in several files.
- Data-loading patterns are inconsistent (`head()` vs `describe()` vs `skim()` vs `summary()`, in varying combinations).

## 4.4 Consider adding cross-references where they would help

Separate from fixing the broken ones in 2.1. Three example threads run across chapters and are the places where students lose continuity:

- Football goals: Ch 8 → Ch 9 → Ch 10 → Ch 12
- The music study (see Priority 5): Ch 7 → Ch 11 → Ch 12
- Heart/smoking/biking data: Ch 4 → Ch 5 → Ch 6

This is an improvement rather than a repair. Do not start it until Priorities 1–3 are done.

---

# Priority 5 — Replace the Ed Sheeran study

**Decided.** Replace with a Christmas music study. Do not begin until Priorities 0–3 are complete — this touches three chapters, two data files and the slides for two lectures, and a half-completed swap is worse than either end state.

## 5.1 The rationale

The current example is a joke that loses cultural currency. It was Bono, then Ed Sheeran, and Sheeran is already fading. Christmas music is permanently irritating to a large number of people, so the currency problem disappears rather than being deferred. It also supports being written as a plausible exploratory study rather than a gag, which fixes most of what currently reads as contrived.

## 5.2 The design — two studies, in sequence

This is the key structural change. The existing two-group t-test and three-group ANOVA become **Study 1 and Study 2 of the same research programme**, not one dataset sliced two ways.

**Study 1** (Ch 7, t-tests): silence vs Christmas music. Participants complete a mildly tedious task — proofreading or data entry — with background audio, told the study concerns concentration and background noise. Irritation measured at baseline (T1) and after exposure (T2).

- The **independent-samples** test compares the two groups at T2.
- The **paired** test is the Christmas group's own T1 vs T2. This replaces the current arrangement, where `IND` and `PAIR` are unrelated datasets — a large part of why it currently reads as contrived.

The paired/independent relationship is itself teachable and should be drawn out: if irritation rose from T1 to T2, why is that not enough? Because the task is boring, so irritation might have risen anyway. That is the cleanest available motivation for why the between-subjects comparison is needed, and it belongs up front, not mid-example.

**Study 2** (Ch 11, ANOVA): silence vs Christmas music vs matched non-Christmas pop, matched on tempo and familiarity.

Study 1's confound is deliberate and should be named at the end of Ch 7: the design cannot separate "background music is irritating" from "Christmas music specifically is irritating", however clean the t-test looks. Ch 11 then opens by picking that up — the third arm exists to answer a question the student is already holding. This gives Lecture 4's causation material a callback in a chapter, which it currently has nowhere.

## 5.3 Measurement

Replace the single hand-entered anger score with a four-item composite, 1–7 agreement: irritated / annoyed / tense / on edge. Composite by mean.

Two reasons. It gives Nick a genuine measurement hook — the outcome becomes a construct measured with error rather than a number someone typed in, which connects back to the measurement material he cares about and currently cannot point at anywhere in the book. And it removes the hand-entered look for free, because a mean of four integers lands on 4.25, not 4.

Keep item-level responses in the spreadsheet and composite in R so the step is visible. Add one sentence on reliability. Do not expand this into a measurement tutorial — house style rule 5 applies.

## 5.4 Data specification

One file, three sheets:

| File | Sheet | Contents |
|---|---|---|
| `Data/MUSIC_STUDY.xlsx` | `S1_IND` | Study 1, both groups, T2 composite + items, `GROUP` |
| | `S1_PAIR` | Study 1 Christmas arm, `IRRIT_T1` / `IRRIT_T2` + items |
| | `S2_ANOVA` | Study 2, three groups, composite + items, `GROUP` |

Variable names: `IRRIT`, `IRRIT_T1`, `IRRIT_T2`, `GROUP`. **Name nothing after Christmas.** Keep every Christmas reference in prose and group labels only, so the next swap is a prose edit rather than an archaeology exercise.

**Two hard constraints.**

*Keep n = 15 per group.* Ch 12's power lesson depends on the study being underpowered — `pwr.anova.test` returning ~50 per group is the entire point of "you can see that my study was rather underpowered". Scaling the data up to look more realistic would destroy it. Small n is plausible for an exploratory study anyway, so this costs nothing in realism.

*Preserve the pattern of results, not merely the significance.* Ch 11's post-hoc discussion needs all of:

- a significant ANOVA with a substantial effect size
- Christmas vs control significant
- Christmas vs other music significant
- **control vs other music failing, with a Tukey confidence interval that only just includes zero**

That last one is load-bearing. It is what lets Nick argue that the interval is a better way to think about the result than the cutoff.

Starting spec, to be tuned against an actual run: control ≈ 3.1, other music ≈ 3.9, Christmas ≈ 4.9, within-group SD ≈ 0.9. Study 1 should give a clear, comfortable t-test result — that chapter introduces the technique, so ambiguity there is a distraction. Study 2 carries the interesting near-miss.

Generate, run, check against the criteria above, and iterate. Do not hand the numbers to Nick until the pattern holds.

## 5.5 Downstream

- `06-t-test.Rmd` (Ch 7): both t-tests, plus the new confound note at the end.
- `10-ANOVA.Rmd` (Ch 11): ANOVA, post-hoc tests, plus the Study 1 → Study 2 opening.
- `11-Issues_w_sig.Rmd` (Ch 12): multiple comparisons discussion and the power calculation. **The effect size will change.** The existing aside about "if I had increased the effect size in the calculation to 0.5" must be updated to whatever the new data actually yields — do not leave the old number.
- Delete `Data/SHEERAN_T.xlsx` and `Data/SHEERAN_ANOVA.xlsx` once the new file is verified.
- **Slides are out of scope for this repo** but must change too. Ed Sheeran appears 13 times in the LFD6 transcript and 20 times in LFD8. Flag to Nick when the chapters are done.

---

## 2.8 Replace the embedded Shiny app — `11-Issues_w_sig.Rmd` (Ch 12)

The familywise error rate calculator is embedded with `knitr::include_app("https://danielroelfs.shinyapps.io/FWER_simple/")`. This writes an iframe at render time and never contacts the app, so a successful render says nothing about whether students will see anything. As of September 2026 the embed is erroring in the browser.

Two problems. The free shinyapps.io tier sleeps when idle, so cold starts fail or stall. And Posit is retiring shinyapps.io — free accounts migrate automatically on 28 January 2027. **The app belongs to Dr Daniel Roelfs, not Nick**, so migrating it is not within Nick's control and the URL may or may not survive.

**The fix.** Replace the iframe with a self-contained figure. Familywise error is `1 - (1 - alpha)^n`, so a plot of FWER against number of tests, for alpha at 0.05, 0.01 and 0.001, reproduces the teaching point with no external dependency.

Annotate the two values used in the lecture: 3 comparisons at alpha = 0.05 (0.14, the post-hoc case), and 26 comparisons at alpha = 0.05 (0.73, the football goals case). These are the numbers Nick reads off the app live, so they must be legible from the figure.

**Keep the attribution.** The idea and the original app are Roelfs's and he gave permission for their use. Retain the credit and the link to `https://danielroelfs.com/about/`, rewritten to acknowledge the app as the source of the approach rather than as an embedded element.

Nick continues to use the live app in lectures for as long as it works. This change only removes the book's dependency on it.

## 2.9 Audit external links

Prompted by 2.8. The book links out to footballhistory.org, premierleague.com, Roelfs's site, and others. `12-references.Rmd` states links were "all checked as of January 2024". Check all external URLs resolve and report any that don't — do not silently substitute replacements.

---

# PART TWO — IF TIME ALLOWS

Start only once Priorities 0 to 5 are complete and merged. This is the highest-value single addition available and the smallest of the three uncovered-lecture items. If the revision runs short of time, it is the one to keep.

## 6.1 Why the book doesn't cover Lectures 1 and 4

Lecture 1 ("Facts and Feelings") and Lecture 4 ("What is a Cause and How do you Know?") are the module's conceptual lectures. Nick says explicitly in Lecture 4 that it is "the last lesson where we don't really talk about any actual analysis". `index.Rmd` tells students the book does not cover them.

That framing is correct for the lectures. It is not a reason for the *book* to stay silent, because both lectures contain material that demonstrates far better than it argues.

## 6.2 The renumbering constraint — applies to everything in Parts Two and Three

Lecture 4 sits between Lecture 3 (Ch 3) and Lecture 5 (Ch 4). A new chapter file for it would render as Chapter 4 and push every later chapter down one, invalidating every slide and spoken reference from Lecture 5 onwards. The same applies to any new chapter for Lecture 1 material.

**Therefore: add sections to existing chapters. Do not create new chapter files.** This gets the content in with no disruption to numbering. Do not revisit this decision without checking it with Nick first.

## 6.3 Demonstrate confounding and selection bias — `03-assoc_rel.Rmd` (Ch 4)

**Why this one.** Three reasons, in order of weight.

Chapter 4 already raises the correlation/causation problem and then moves on, leaving students with an assertion and nowhere to go.

The Study 1 → Study 2 structure introduced in Priority 5 is itself a causal argument — Ch 7 ends by noting the two-arm design cannot separate "background music is irritating" from "Christmas music is irritating", and Ch 11 resolves it. That argument currently has no chapter to reference. This section gives it one, and should be cross-referenced from both.

Confounding is the most simulable concept in the module. Simulated data where X and Y correlate strongly with no causal path between them — and where the correlation collapses once Z is conditioned on — lets a student watch the phenomenon rather than take Nick's word for it. That is roughly fifteen lines of code and is worth more than several paragraphs of argument.

**Scope.** A demonstration of confounding and selection bias. **Not** a causal inference chapter. Lecture 4 mentions difference-in-differences, instrumental variables, propensity scores, natural experiments and DAGs — naming these as things that exist is fine, teaching them is not. It would break house style rule 5 and duplicate the advanced quantitative modules, which is explicitly not this module's job.

**Content.**

1. Simulate a confounded relationship. Two variables correlating at roughly 0.6 with no causal path between them, both driven by a third. Show the correlation. Then show it vanishing once the confounder is conditioned on.
2. A short selection bias demonstration. Generate a population with no relationship between two variables, then select a subsample on a combination of both, and show a relationship appearing from nothing. This is the more surprising of the two and is worth the space.
3. A brief closing passage on why randomisation addresses this, in conceptual terms, linking forward to the music study.

**Placement.** After the correlation material, before regression. This positions it as "here is why a correlation isn't enough" immediately after students have learned to compute one.

**Acceptance criteria.** No new statistical machinery. No causal inference techniques taught. Every claim demonstrated by simulation rather than asserted. Seeded so the numbers are stable within a render, with the usual note that they differ from the slides. Cross-referenced from `06-t-test.Rmd` and `10-ANOVA.Rmd` with `\@ref(assoc_rel)`.

---

# PART THREE — DEFERRED

Not to be started until Parts One and Two are complete. Recorded now so the reasoning and the placement decisions are not lost.

## 7.1 Risk communication — `01-intro.Rmd` (Ch 2)

**Source.** Lecture 1 contains substantial material on risk that is arithmetic rather than philosophy: relative versus absolute risk, base rates, and the error of reading a percentage-point increase as a percentage increase. The worked example is the processed meat and colon cancer risk story.

**Why it belongs in the book.** This is arguably the most transferable skill in the module for the students Nick describes as unlikely to do quantitative research themselves but certain to read it. It is also the one piece of Lecture 1 that is genuinely quantitative and therefore genuinely at home in a textbook.

**Why Chapter 2.** That chapter is already about how presentation of data misleads — truncated axes, chosen baselines, selective display. Relative risk is the same lesson expressed in numbers rather than pictures, so it extends the existing argument rather than interrupting it.

**Scope.** Short. The worked example with real numbers, the base rate point, and the population-versus-individual distinction Nick draws in the lecture. Do not expand into epidemiology or risk perception literature.

## 7.2 Simulate p-hacking — `11-Issues_w_sig.Rmd` (Ch 12)

**Source.** Lecture 9 covers p-hacking, the garden of forking paths, the replication crisis, pre-registration and the power pose case. Chapter 12 already discusses these in prose.

**The addition.** Simulate it. Generate data containing no real effect, run a set of defensible-looking analytic choices across it — subgroup splits, outlier rules, alternative outcome codings — and show a significant result emerging. A p-curve across many such runs makes the point visually.

**Why it's worth the effort.** It is the single most persuasive demonstration available in the module. A student who watches a publishable "finding" assemble itself out of pure noise does not need to be told the replication crisis is real. It also directly reinforces the book's existing position on thresholds and significance stars.

**Why it's deferred.** It is the largest of the three additions, and Chapter 12 is already the most heavily revised chapter in Part One thanks to the Priority 5 downstream changes. Do not touch it until those are stable.

**Placement.** A section within Chapter 12. No new chapter, no renumbering.

---

# Out of scope — flag to Nick, do not action

Two points surfaced from the LFD8 transcript that are slide-level rather than book-level:

- Nick corrects himself live on a bullet point about false positive rates: "that's actually incorrect, that bullet point, you know, I have to change that."
- The p-value is described in the lecture as "the chance of a false positive, literally", which sits awkwardly against the more careful treatment in Ch 10.

Neither requires a change to the textbook. Both are worth Nick's attention before the 2026 run.
