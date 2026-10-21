# Milestone 1 — reading notes

## Community

**Hacker News comments** (news.ycombinator.com) — public, no login, and a lot of
text with a wide spread of quality, from a single sentence of reaction up to
paragraphs with numbers in them.

I first looked at a sports subreddit, but Reddit returns 403 to anything that
isn't a logged-in browser, so I couldn't read it from here. Hacker News has an
open API (`hn.algolia.com/api/v1`), which also gives me a clean way to collect
200 posts later.

A post here means a **top-level comment** on a front-page story (50+ points,
40+ comments). I'm not using replies, because a reply can only be understood
next to the comment above it.

## How I read

I read 40 top-level comments, one per story, from 40 different threads on
different topics (laptops, an obituary, a lawsuit, an AI release, a GitHub
pricing change, etc.). I drew them at random so I wouldn't just read the ones
that stood out. Comment ids are below so the reading can be checked.

`26290787 48510899 16590905 18135522 27422960 26719995 23860020 22934889
39543429 19740998 42875171 6210374 29877759 47236983 18855071 22879465
21877345 46423916 44262985 13742687 23378791 41402920 42811809 18011745
12719616 48992390 3080138 37026829 40725777 18228305 18146365 23389827
40371224 47116195 48537762 18354825 36263309 26948397 48769730 22883584`

## What I noticed

Rough sort of the 40, by what the comment is doing:

| Count | What the comment does | Examples (HN id) |
|---|---|---|
| ~6 | Backs a point with a specific detail that carries the argument | 48510899 (16 hours with a model, one hour lost to a git rebase/stash failure, code gone); 27422960 (TA caught two near-identical printouts, both penalised); 18855071 (git-lfs question with Azure DevOps as comparison); 22879465 (10k to 3k free Action minutes); 46423916 (gamma correction explained as what displays expect) |
| ~10 | States a confident view and offers nothing to check it | 26290787 ("This is the wrong way to go."); 23860020 (hacked accounts were fake accounts run by Twitter staff); 18146365 ("Had China been weaker, it would have been invaded already"); 29877759 ("what an elitist idea… Yuck."); 48769730 (Anthropic would "rather plant a Trojan") |
| ~9 | Feeling or social response, no claim | 3080138 / 18228305 / 37026829 (RIP comments); 41402920 ("this warmed my frigid heart"); 42811809 ("Extremely impressive work!"); 48992390 (thanks, I'm a bowler); 22883584 ("This is pretty cool") |
| ~15 | Neither: a question, a link, a quote, meta chatter about HN itself, or a reporter looking for sources | 39543429 ("Who is Bartosz Ciechanowski?"); 44262985 (bare link); 16590905 (a Hawking quote); 12719616 ("4 votes away from the top 10"); 18354825 (Bloomberg reporter asking for contacts) |

That sort is by eye and I'd expect some of the 40 to move if I did it again.
The 15 in the last row is the thing I did not expect. Going in I'd assumed
"good comment / bad comment" would cover it, and a third of what I read isn't
either.

## Three candidate distinctions

Each is something I can point at in a specific comment.

1. **Does it contain a specific, checkable detail that does work in the
   argument?** A number, a named product or version, a date, a concrete thing
   that happened to the writer. 48510899 and 22879465 do; 26290787 and
   18146365 don't. Most comments don't. Only about 6 of 40 did.
2. **Is it a claim, or a feeling?** Some comments assert something about the
   world, whether or not they support it (23860020, 29877759). Others only
   express how the writer feels about an event (the RIP comments, 41402920).
   Both are confident in tone, so tone doesn't separate them. What separates
   them is whether anyone could disagree with the comment.
3. **Is it a contribution to the discussion at all?** Questions, bare links,
   quotes and notes about the site make no point either way (39543429,
   44262985, 12719616). Any label scheme has to decide whether these get a
   label of their own or are filtered out before labeling.

A fourth one I noticed but am not sure about: **does the writer draw on
personal experience** (27422960, 48510899)? It overlaps with distinction 1
heavily, because the experience is usually what the detail is.

## What this means for the next milestone

- Distinctions 1 and 2 together give a natural three-way split (supported
  claim / unsupported claim / feeling).
- Distinction 3 is a decision to make before collecting: roughly a third of
  random top-level comments fall outside those three. If I label them all, the
  catch-all label could pass the 70% cap on its own. If I filter them out, I
  have to write down the filter so the 200 are drawn consistently.
- Supported claims were rare (~15%), so the thinnest label will be that one.
  I should collect with that in mind, not just sample at random.
