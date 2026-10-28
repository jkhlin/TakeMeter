# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

**Community:** Hacker News (news.ycombinator.com). A "post" is a top-level
comment on a front-page story. It's public, needs no login, and the quality
varies a lot, from one-line reactions to paragraphs with numbers in them.
Reddit blocked automated access, so I used Hacker News's open API instead.

**What it sorts:** each comment into `analysis` (a point backed by a specific,
checkable detail), `hot_take` (a confident claim with nothing checkable behind
it), or `reaction` (a feeling about the story, with no claim).

**Reading notes (Milestone 1):** I read 40 random top-level comments. About 6
backed a point with a specific detail, about 10 made a claim with nothing
behind it, about 9 were only a feeling, and about 15 were none of those
(bare questions, links, quotes, chatter about HN itself). That last group is why
the scope rule below exists. HN ids read: 26290787 48510899 16590905 18135522
27422960 26719995 23860020 22934889 39543429 19740998 42875171 6210374 29877759
47236983 18855071 22879465 21877345 46423916 44262985 13742687 23378791
41402920 42811809 18011745 12719616 48992390 3080138 37026829 40725777 18228305
18146365 23389827 40371224 47116195 48537762 18354825 36263309 26948397
48769730 22883584.

**Scope rule:** I only collect comments that make a point or express a feeling.
I skip a comment if its whole content is a question, a bare link, a quote with
no comment of the writer's own, a bare recommendation or pointer ("check out
X", "use Y instead"), or chatter about HN itself (votes, rankings, moderation).
The recommendation clause was added in Milestone 4, after it came up in the
first random draw.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `analysis`

**Definition:** Makes a point and backs it with a specific, checkable detail
that the main claim depends on: a measurement, a date, a version, a named
product, person or event, or something concrete the writer did or saw.

**Example 1:** (HN 48510899)
> The people yelling "Fable will take your job" are still at the first meeting.
> I have used it for 16 hours, and spent one of those hours fighting it over
> git. It rebased wrong, stashed changes, forgot it had stashed them, merged a
> stash it claimed did not exist, then reset to HEAD. By the end, I had lost
> the code we had just worked on.

**Example 2:** (HN 27422960)
> I TA'ed a Comp Sci class a long time ago (back when they had to submit their
> code as print-outs). […] I went back through the others and found another one
> that was almost exactly the same. Took it to the Prof and we agreed the ones
> that copied got 0 on the project, and the ones who allowed the copying got 50%
> of their mark. Honestly, they got off quite easy.

### `hot_take`

**Definition:** Asserts a view about the world, a company, a person, or what
should happen, and offers nothing checkable for the main claim. It might be
true. It isn't argued.

**Example 1:** (HN 18146365)
> As long as you are powerful, you can get away with almost anything. This goes
> from individuals to countries. Had China been weaker, it would have been
> invaded already.

**Example 2:** (HN 26290787)
> This is the wrong way to go.
> Put together a battery, monitor, keyboard and pointer in a foldable package,
> with an open space to drop-in and connect one of the many mini PCs out there.
> The "laptop" part could last for decades, while the customer just swaps-out
> the mini PC inside it every couple years to upgrade.

### `reaction`

**Definition:** Expresses the writer's feeling about the story (thanks, praise,
sympathy, RIP, excitement, disgust) without asserting anything about causes,
other parties, or what should happen.

**Example 1:** (HN 41402920)
> this warmed my frigid heart. so glad to see this sort of curious spirit
> endures :)

**Example 2:** (HN 18228305)
> Can you imagine if we could have another ~30 years of Allen? Me neither. I
> really hate cancer. RIP space guy.

### The hardest boundary

**Which two labels:** `analysis` and `hot_take`.

Hacker News comments often contain a number or a name that is incidental, or a
detail that supports a side point and not the headline. The post that sat
closest to the line in my reading was HN 47236983:

> Motorola seems to HATE updating their operating systems. They only support
> their phone OS for the minimum possible time. Moving to an open source
> platform makes way too much sense for them. […]

It sounds factual, and "the minimum possible time" could in principle be
checked, but no figure, date or version is given.

**The decision rule I used every time:**

1. Find the comment's main claim: the thing the comment is *about*.
2. Underline the most specific thing offered for it.
3. It's `analysis` only if the underlined thing is a **particular** checkable
   thing (a measurement, a date, a version, a named event, something the writer
   did or saw) **and it bears directly on the main claim.**
4. Otherwise it's `hot_take`. That includes: a generic pattern ("they always
   do this"), a rhetorical figure ("sunset in 18 months"), a detail that only
   supports a side claim, and a vague gesture at evidence ("they only support it
   for the minimum possible time").

The Motorola comment is `hot_take`.

**Second boundary, `hot_take` vs `reaction`:** praise, thanks or sympathy about
the thing itself is `reaction`. A claim about *why* something happened, about
someone else, or about what should happen is `hot_take`, even if the comment is
mostly feeling. A comment that is both is judged on its main point.

**A label I dropped:** a fourth label for questions, bare links and meta
chatter. About 15 of my 40 comments were that. It wouldn't hold as a label
because it is defined by absence (it isn't a claim and it isn't a feeling), so
it would have swallowed anything I was unsure about. I made it a scope rule
instead (see "What This Does").



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Top-level comments on 330 Hacker News front-page
stories (120+ points, 40+ comments) from 2012 to 2026, pulled through the public
HN Algolia API (`hn.algolia.com/api/v1`), up to two random comments per story.
That gave a pool of 532. After my scope rule I read through about 320 of them in
random order. Comments over 1,500 characters were excluded, not trimmed. Every
row in `labels.csv` has its HN comment id in the `note` column.

**How I labelled them:** All 210 rows were labelled by Claude against my
definitions in this README and `label_definitions.txt`, and I then read every
one and changed none. I did not do the unaided cold check the project describes.
- **190 rows:** `note` says `pre-labelled, checked`. 47 carry a `HARD:` note
  saying which two labels the post sat between.
- **20 rows, drawn at random before any labelling to be the cold set:** I had
  Claude label these too instead of labelling them myself. I read all 20 and
  agreed with every label. Their `note` says `pre-labelled, checked; was the cold
  draw but not labelled unaided`, and 3 carry a `HARD:` note. They are marked that
  way so nobody reads them as an unaided check of my own labelling. Because they
  were read after Claude labelled them, they don't test my consistency. The
  staff-set agreement check in unit 6 is the first thing that does.
- **Not purely random:** after the first 220 comments gave me plenty of
  `hot_take` but too little `analysis`, I added comments from later in the pool
  for `analysis` and `reaction` only, so that no label would fall below 20% or
  above 50% (my criterion 4).

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `analysis` | 53 | 25.2% |
| `hot_take` | 92 | 43.8% |
| `reaction` | 65 | 31.0% |
| **Total** | 210 | 100% |

The largest label (`hot_take`, 43.8%) is under the project's 70% limit and under
my own 50% limit (criterion 4). The smallest (`analysis`, 25.2%) is above my 20%
floor.

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

*These three were picked from the hard cases Claude flagged during
pre-labelling. Swap in your own if reviewing turns up better ones.*

**1.**
> *The post:* "My main gripe is lack of memory. Since it has no sd card, it
> should have at least 128GB like the iPhone 6." (HN 8477206)
>
> *Could have been:* `analysis` or `hot_take`
>
> *I chose:* `analysis`, because it names a figure (128GB) and a particular
> comparison (the iPhone 6) that bear directly on its one claim. It's close:
> it's a one-line gripe, and under the rule's "particular and bears on the main
> claim" test it passes narrowly.

**2.**
> *The post:* "Comodo's behavior is shameful. How can we punish them? Is there a
> way to move away from using their services?" (HN 11963059)
>
> *Could have been:* `reaction` or `hot_take`
>
> *I chose:* `hot_take`, because "shameful" is a claim about another party and
> not a feeling about the story. This is the second boundary in my rule
> (`hot_take` vs `reaction`) and outrage is where it bites.

**3.**
> *The post:* "You.com already lists Llama 3 as one of their available LLMs."
> (HN 40098989)
>
> *Could have been:* `analysis`, or out of scope
>
> *I chose:* `analysis`, because it's a specific, checkable fact (a named
> product and model) and the fact is the whole point of the comment. The
> doubt is whether a bare fact with no argument around it is a point at all, or
> just information.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`, fine-tuned for 3-way classification
(`analysis`, `hot_take`, `reaction`).

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, 128 tokens per post,
seed 42.

**Trained on:** CPU (no GPU), torch 2.14.1+cpu. `results.json` records this as
`"device": "cpu"`. Training took about 90 seconds.

**Anything I changed from the defaults, and why:** Nothing. All settings are the
notebook's defaults. I ran sections 1 to 5 once, with Claude executing the
notebook's code cells in order from a script using the project's `.venv`, not by
clicking through the notebook. The notebook file itself wasn't changed, so it
has no saved outputs from this run. The dataset check printed `Data looks fine`
with no `[FIX ME]` or `[look at]` lines.

**Split sizes:** 146 train / 32 val / 32 test, stratified 70/15/15 on seed 42.

| Label | Train | Val | Test |
|---|---|---|---|
| `analysis` | 37 | 8 | 8 |
| `hot_take` | 64 | 14 | 14 |
| `reaction` | 45 | 10 | 10 |

No label has fewer than 8 in the test split, but `analysis` has exactly 8, and
the notebook's own threshold for a jumpy metric is "fewer than about 8". One
wrong prediction on `analysis` moves its recall by 12.5 points, so its F1 will
swing between seeds in unit 6. (My criteria assumed about 6; it's 8, so the
estimate was a little low.)

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* A fresh AI agent, given only my three definitions and not my examples, to write eight Hacker News comments sitting on the boundaries (3 analysis/hot_take, 3 hot_take/reaction, 2 analysis/reaction), without saying what the right label was.
- *What came back:* Eight realistic comments. Using my first-draft rule ("a specific detail, and removing it leaves the conclusion bare") I could classify five cleanly and hesitated on three. (1) "SQLite is the right answer for 95% of projects… I've run a 40GB file for two years without corruption": the anecdote is concrete but supports only a side claim, not the 95%. (2) "a beloved product getting sunset in 18 months": a number, but a rhetorical one. (3) "The migration guide alone is worth the price of admission": praise or claim?
- *What I changed:* Added three clauses to the decision rule: the detail must bear on the *main* claim; it must be a *particular* thing, not a generic pattern or a rhetorical figure; and praise about the thing itself counts as `reaction`. I wrote the definitions and classified the eight myself, so I knew what I meant by them. Re-test with a second person before trusting this.

**Moment 2**

- *What I asked for:* Claude to pre-label a pool of Hacker News comments against my three definitions and my decision rule, skip anything out of scope, and note each one that sat between two labels.
- *What came back:* Labels for about 275 in-scope and out-of-scope comments, 190 of which went into `labels.csv`, with a `HARD:` note on 47. It was consistent about the rule, but a pattern showed up: it kept hesitating between `analysis` and `reaction` on first-person anecdotes with a concrete detail, such as a son's surgery or flying to Australia for a Stripe account.
- *What I changed:* I read all 210 and changed none of the labels. `labels.csv` still matches what Claude wrote row for row, so every judgment call on the 50 hard cases stayed as Claude made it. That is worth checking, because it could mean the definitions are clear, or that I accepted Claude's reading of them. I had Claude label the 20 cold-draw rows as well, so there is no unaided check of my own labelling yet.

**Pre-labelling disclosure:** All 210 rows in `labels.csv` were labelled by
Claude against my definitions. I read every one afterwards and agreed with every
label; the 190 main rows are marked `pre-labelled, checked`. The other 20 rows
were the random draw meant for my unaided cold check. Claude labelled those too,
so they are marked `pre-labelled, checked; was the cold draw but not labelled
unaided`. The posts were also collected by Claude, through the HN API.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.281 | 0.625 | +0.344 |
| Macro F1 | 0.181 | 0.483 | +0.302 |
| F1 — `analysis` | 0.410 | 0.000 | -0.410 |
| F1 — `hot_take` | 0.133 | 0.700 | +0.567 |
| F1 — `reaction` | 0.000 | 0.750 | +0.750 |

Both columns are scored on the same 32 held-out posts (`test_split.csv`). The baseline uses the definitions in `label_definitions.txt`. A bare-label-names run scored higher (accuracy 0.375, macro F1 0.295), so my definitions didn't help the zero-shot model.

**What I predicted before I looked:**
I expect the trained model to beat the baseline most on `reaction` and `hot_take`, where the zero-shot model scored 0.000 and 0.133 F1 with my long definitions, because fine-tuning learns what those posts look like instead of matching them against a sentence-long definition, and the gap to be smallest on `analysis`, where the baseline was strongest (0.410).

*Note: this prediction was drafted by Claude from the baseline numbers, after it had already opened `results.json` while checking the repo, so it isn't a fully blind prediction.*
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

**Three seeds, same 210 posts, same settings** (`results_three_seeds_before.json`).
All three ran on `cpu` (`CPU`), torch 2.14.1+cpu, `max_length` 128, as recorded
in that file. Each seed's test split is 32 posts (8 `analysis`, 14 `hot_take`,
10 `reaction`). As in the training run above, Claude ran section 6 from a script
using the notebook's own cells, not by clicking through the notebook.

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.625 | 0.438 | 0.438 | 0.188 |
| Macro F1 | 0.483 | 0.203 | 0.203 | 0.280 |
| F1 — `analysis` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `hot_take` | 0.700 | 0.609 | 0.609 | 0.091 |
| F1 — `reaction` | 0.750 | 0.000 | 0.000 | 0.750 |

Accuracy moved 18.8 points across seeds, well past the notebook's 10-point line,
so by its own rule this is too unstable to trust. The cause is visible in the
numbers: on seeds 7 and 2024 the model called all 32 test posts `hot_take`, and
0.438 is exactly 14/32, the `hot_take` share of the test split. Only seed 42
got `reaction` posts right (6 of 10). Training loss on every seed ended around
0.94 to 0.97, close to chance for three labels (ln 3 ≈ 1.10), so with 146
training posts and 3 epochs the model is barely past guessing the majority
class. Seed 42's 0.625 was a friendly draw, not the typical result.

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

Seed 42 (the notebook's section 7 prints this seed). Rows are true labels,
columns are the model's guesses.

| true \ predicted | analysis | hot_take | reaction |
|---|---|---|---|
| **analysis** | 0 | 8 | 0 |
| **hot_take** | 0 | 14 | 0 |
| **reaction** | 0 | 4 | 6 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->
All 8 real `analysis` posts were called `hot_take` and none went the other way,
so the model never learned the `analysis` / `hot_take` boundary and treats every
`analysis` post as a `hot_take`; seeds 7 and 2024 show the same thing on every
post, so this is a model that defaults to the majority label rather than one
confusing a specific pair.



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 30 / 30 = 100%

**Who labelled these:** Claude, not me. I asked it to do Milestone 3, so it read
`data/staff_taxonomy.md`, labelled the 30 posts in `data/staff_posts.csv` under
those rules (10 `analysis`, 10 `hot_take`, 10 `reaction`), and saved
`my_staff_labels.csv` before `agreement.py` read the staff labels. This rate
measures how well Claude applied the staff rules. It says nothing about how
consistently I applied my own taxonomy to my 210 posts, which is what this
check is meant to tell me.

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

There were none, so there is nothing to adjudicate and no (a)/(b)/(c) piles to
fill. `agreement_results.json` records 30 agreed, 0 disagreed.

**What the pattern in my disagreements tells me:**

With no disagreements there is no pattern to read, and 100% is not by itself
evidence that the staff rules are tight. The taxonomy says staff picked several
posts for being ambiguous and that a rate in the 70s is normal, so a perfect
score more likely means the set was easier than advertised, or that labels from
a model that has read the decision rules line up with staff more than a person
working from memory would. The closest calls, where the rules had to do the
work, were these, and staff agreed on every one:

- Posts 14 (`hot_take`) and 17 (`hot_take`): each has a number or date (3% since
  the shoe rules changed; "peaked in about 2015"), but the fact decorates a
  conclusion already stated ("so obviously...", "everything since has been
  marketing"). Rule 1, and the 3% example in the taxonomy is nearly post 14.
- Post 18 (`hot_take`): "slower than 8 minute miles" is a threshold in the claim,
  not a fact that carries an argument.
- Posts 5 and 10 (`analysis`): each is one runner's own numbers, but the numbers
  are the reason for the conclusion (rule 1), so they count even though the
  sample is tiny.
- Posts 27 and 29 (`reaction`): both contain a concrete detail (20 degrees of
  lean, the wrong-size shoes), but the post is about what just happened to the
  writer and the detail isn't argued from (rules 2 and 3).

If I want a real consistency check for Milestone 4, I should label these 30
myself, or relabel a sample of my own 210 posts without looking at the existing
labels.



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
