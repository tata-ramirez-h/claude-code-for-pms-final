---
name: review-checklist
description: Reviews a product brief against four checks: it names who owns it, says how we'll know it worked, keeps the same scope from start to finish, and explains the problem before the fix. Use when the user points at a brief (a file path or pasted text) and asks to review it, check it, or run the review checklist.
---

# Review checklist for briefs

The user points you at a brief. Read all of it, then check it against the four tests below. Judge only what is written in the brief. Do not fill gaps from other files, memory or what the author probably meant; if the brief doesn't say it, it fails.

## The four checks

### 1. Names who owns it
- Pass: a named person, or a clearly identified role, is accountable for the work and for the decision to ship, stop or change it.
- Weak: several people are mentioned but none is marked as the owner, or the owner is a team ("Engineering").
- Fail: no owner, or only "we".
- Note people who are asked for input (for example for size or time) separately: consulted is not owning.

### 2. Says how we'll know it worked
- Pass: at least one measure, with where it is read from, a starting value or a way to get one, a target or direction, and when it will be read.
- Weak: a measure is named but with no baseline, target or date ("improve acceptance rate").
- Fail: no measure, or only activity ("ship the change", "gather feedback").
- Also look for a stop or roll-back condition. Its absence is worth a note, not a fail.

### 3. Keeps the same scope start to finish
Compare what the brief promises in its opening (title, summary, problem) with its middle (proposal, requirements) and end (success measure, next steps, asks).
- Pass: the same thing is described throughout; anything added or dropped is called out.
- Weak: wording drifts but the substance matches.
- Fail: something appears in one part only. Typical cases: the success measure covers a different group than the problem, the proposal solves more or less than the problem stated, the summary and the details disagree, or an item in the asks was never introduced.
- Quote the two passages that disagree.

### 4. Explains the problem before the fix
- Pass: the problem, who has it and what the evidence is come first, and the solution appears only after.
- Weak: the problem is there but the fix is mentioned first, or the problem is stated only as the absence of the fix ("we need X").
- Fail: no problem is stated, or the problem is not tied to anyone who experiences it.
- Check that the fix actually follows from the stated problem.

## Output

Start with a one-line verdict: how many of the four pass, and whether the brief is ready to share or needs another pass.

Then a table:

| Check | Result | Evidence |
|---|---|---|
| Owner | Pass / Weak / Fail | short quote or "not stated" |
| Success measure | ... | ... |
| Consistent scope | ... | ... |
| Problem before fix | ... | ... |

Then, for each check that is not a pass, one concrete fix: the sentence or section to add or change, written so the author can paste it in. Where the fix needs a fact you don't have (a name, a number, a date), write a placeholder like `[owner: name or role]` and say who would know. Never invent one.

Keep the whole review under one page. Don't rewrite the brief and don't edit the file unless the user asks.
