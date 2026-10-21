# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall accuracy

On the held-out test split, overall accuracy is at least **0.70** on each of the
three seeds.

**Why this target:** Three labels means guessing gets about 0.33, and always
guessing my biggest label gets at most 0.50 (criterion 4 caps it there). 0.70 is
about seven right in ten: good enough to use as a first-pass sort of Hacker News
comments, wrong often enough that I'd still check the ones that matter. The test
split is only 30 posts (15% of 200), so one post is 3.3 points and the line is
21 of 30. I expect seed-to-seed swings of around 10 points on that few posts,
which is why I'm not setting it higher.

---

## 2. Per-label F1

On the test split, every label has an F1 of at least **0.60** on each of the
three seeds. The label with the fewest examples in `labels.csv` is held to
**0.50** instead.

**Why this target:** An accuracy of 0.70 can hide a label the model never
predicts, so I want a number per label. I expect `analysis` to be my thinnest:
only about 6 of the 40 comments I read had a specific checkable detail, and even
if I collect for it on purpose it will probably end up around 40 examples, so
about 6 in the test split. One wrong prediction out of 6 moves that label's F1 by
roughly 0.1, so it gets the looser 0.50. The other two labels should have around
10 test examples each, enough to hold to 0.60.

---

## 3. Consistency

When I label the 30 posts in `data/staff_posts.csv` under the staff definitions,
I agree with the staff labels on at least **24 of 30 (0.80)**.

**Why this target:** This checks whether I can apply a written rule I didn't
write, which is the same skill that decides whether my own 200 labels are
consistent. Chance agreement on three labels is about 0.33, and 0.80 is roughly a
Cohen's kappa of 0.70. Below that, I'm not applying definitions reliably, and
my 200 Hacker News labels are probably noisier than the 0.70 accuracy target in
criterion 1 assumes. The staff posts come from a running forum, not Hacker News,
so this tests my rule-following, not my taxonomy.

---

## 4. Balance

In `labels.csv`, no label is above **50%** of the rows and none is below **20%**
(at least 40 of 200).

**Why this target:** The project's own cap is 70%, but a model that always
guesses a label holding 70% of the data scores 0.70 accuracy, which would clear
criterion 1 while learning nothing. 50% closes that loophole. The 20% floor is
for the test split: 20% of 30 is 6 posts, the fewest I'd trust to compute an F1
on. This isn't free. About 15% of the comments I read had specific support, so
reaching 20% means collecting `analysis` posts on purpose, not sampling at
random.

---

## 5. Confidence

On the test split, for each of the three seeds, accuracy on the most confident
third of predictions is at least **20 points higher** than accuracy on the least
confident third (`accuracy_most_confident_third` minus
`accuracy_least_confident_third` in `results.json`).

**Why this target:** If confidence doesn't track correctness I can't use it to
send unsure comments to a person, which is the obvious use of this model. I'm
comparing thirds, not using a fixed threshold like "above 0.90 is right 90% of
the time", because a small model trained three epochs on about 140 examples may
never get that confident, and a threshold would then be untestable. A third of a
30-post test split is only 10 posts, so one post is 10 points. 20 points means at
least two more right in the confident third than in the unsure one, which a
confidence signal with real information should clear and one that is random
could still hit by luck. That's why it isn't higher.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
