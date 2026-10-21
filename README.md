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
no comment of the writer's own, or chatter about HN itself (votes, rankings,
moderation).

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

**Where the posts came from:**

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->

**Counts per label:**

| Label | Count | Share |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |
| **Total** |  | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**2.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**3.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**

**Settings:** <!-- epochs, learning rate, batch size, seed -->

**Anything I changed from the defaults, and why:**

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->



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

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:**

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

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

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



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

**Agreement rate:** ___ / 30 = ___%

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

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



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
