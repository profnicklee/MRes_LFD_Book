# Optional Add-in — NHS Mortality / Selection Bias

**Status: not currently in scope. Do not action this file.** It's kept here in case Nick decides to add it into the selection-bias section of `03-assoc_rel.Rmd` later. If this file is still sitting in the repo root and nobody has said to use it, ignore it.

## Where it would go

As a third "landing" example in the selection bias demo — after the NBA hook and the hiring deep example, before the closing passage on randomisation. It's written to bridge directly into that closing passage; its last line is deliberately echoed by the passage's opening.

## Draft prose

> Here's a version of this that isn't a classroom exercise, because it's mine, and it isn't finished.
>
> The NHS runs something called the Friends and Family Test — essentially a Net Promoter Score for patients, and separately for staff: would you recommend this ward, this service, to someone who needed the same care? [co-authors / journal — Nick to decide whether to name, or leave as "in some work of mine"] and I found a relationship between hospitals' FFT scores and their mortality rates, for both the patient and staff versions of the measure. It's the kind of finding that should be genuinely useful — an early-warning signal that costs nothing to collect and might flag a struggling hospital before the mortality figures alone would.
>
> Reviewers keep fixating on one objection: maybe this isn't the recommendation score doing anything at all — maybe healthier, more informed patients are selecting into the hospitals that already have better reputations, and that selection is manufacturing the whole relationship. It's a fair question to ask. It just happens not to be true for most NHS care — unlike a lot of healthcare systems, patients mostly don't choose their hospital; they go to the one they're sent to. So we checked directly: we reran the analysis on A&E admissions alone, where there is essentially no opportunity for a patient to select a hospital at all — you go where the ambulance takes you — and the relationship held. That's about as close as you get in real data to ruling a specific selection story out rather than just arguing it away.
>
> It hasn't been enough. The reviewers pushing hardest tend to be from healthcare systems where patient choice of provider is the norm, and the selection story is intuitive to them in a way it simply isn't for the NHS. Being right about your own data doesn't always mean the argument is over.
>
> And to be fair to the sceptics: this fight is real, and it doesn't always go one way. There's a genuine, ongoing dispute in health economics over publishing individual surgeons' mortality rates. One well-known analysis of a decade of New York and Pennsylvania cardiac surgery data found that after public reporting began, surgeons appeared to steer away from their sickest patients — protecting their published numbers by selecting an easier caseload, not by operating better. Other researchers, using different severity measures on the same kind of data, argue the opposite happened. Nobody has fully settled it. Selection bias isn't always a trick you can catch in a classroom exercise and file away — sometimes it's a live argument nobody's won yet, and the only real way out of it is not to argue about it after the fact, but to design the study so selection can't happen in the first place.

## Open items before this could be used

- Co-author names and target journal for the FFT study — Nick's call whether to name it or leave it as "some work of mine," especially while still under review.
- Whether to cite Dranove, Kessler, McClellan & Satterthwaite (2003), *Journal of Political Economy*, "Is More Information Better? The Effects of 'Report Cards' on Health Care Providers," and the *Health Affairs* rebuttal disputing their severity measures, in `book.bib`. Both are real, citable literature — recommended if this section gets used.
- This piece is narrative, not a demonstrated claim, so it doesn't need its own simulation and doesn't affect the "everything demonstrated by simulation" acceptance criterion — but worth re-confirming that reading holds if this gets revived.
- If added, the last line here replaces the opening framing of the closing passage in the main handover doc (they're written to hand off to each other) — check they still read smoothly together rather than dropping this in as a fourth disconnected block.
