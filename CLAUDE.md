# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I am the new PM for **Rook Dispatch** at Rook Industries (fictional). I
joined after the previous PM, Priya, left; we never overlapped. Source for
this section: `00-rook/company/notes/handoff-from-priya.docx` (written
21 Aug 2026), the only file in `00-rook/company/`. It is one person's
view, so treat its claims as hypotheses to check, not facts.

### Products
- **Dispatch** is the flagship, and the thing responders stay for. An
  incident comes in, the system ranks available responders, offers the
  callout to the top one, and they take it or don't. Parts: the
  **console** (handlers use it; stable), the **mobile** app (stable since
  4.1) and **routing** (the interesting work, and the risk).
- The wiki also lists Rook Supply, Bulk Callout, Handler Phone App and
  Requisition Approval Chains. The handoff does not cover them.

### People (roles only; the handoff gives no names)
- **Engineering manager**: runs Dispatch engineering. Candid. First stop
  when unsure, and the person who can pull numbers.
- **Staff engineer**: built the logic that decides who gets pinged. The
  only real source on how a responder is ranked; no document exists.
- **Support lead**: hears handler complaints first. Wants a standing
  15 minutes.
- **Director of Product**: my director. Gives room.
- Names may be in the wiki's Team directory. I haven't matched them yet.

### Vocabulary
- **Callout**: an incident needing a responder. **Responder**: person in
  the field with a phone. **Handler**: person who looks after a responder
  and sits at the console.
- **Ping**: asking one responder's phone to take a callout. Pings go out
  one at a time until someone accepts.
- **Ranking**: ordering available responders. **Acceptance rate**: share
  of pings taken; the number everyone watches, so I must be able to
  explain it. **Ping timeout**: how long a responder has to answer.

### Where things stand
- **4.2 shipped 12 Aug** and is the live problem. Two changes: (1) the
  ranking now weights proximity above recent acceptance history, a
  three-quarter-old request from responders in wide areas; (2) the ping
  timeout was cut.
- Since then, **fewer pings are accepted and more handlers complain**.
  Priya's read: mostly seasonal (August is always soft), recovering in
  September. Not verified. Two changes plus seasonality overlap, so no
  one cause is proven. The database has callouts and pings to check it.
- Priya advises against framing this as "revert 4.2": the change was
  requested, and reverting trades one angry group for another.
- 4.2 also changed console **filter persistence**. Priya calls it
  cosmetic noise that will generate tickets; don't let it dominate.
- Some items were squeezed out of 4.2. Which are still **Q3
  commitments** has not been agreed with the Director of Product. This
  conversation is overdue.
- **No written description of who gets pinged exists.** Priya asked me
  to write one.
- Priya was sole PM for 14 months and "made calls faster than she
  checked them", so unexamined parts of the product may hide bad ones.

### How to help me
- Separate what the data shows from what Priya believed.
- Before blaming 4.2, check the timeout cut and seasonality separately.
- Don't invent names, numbers or decisions. Say when something is not in
  the files.

### 4.2 vs the callout data
Checked against the Rook database. "Before" = 29 Jun–11 Aug, "after" =
12 Aug–6 Sep (the data ends 6 Sep; nothing newer exists). This replaces
Priya's "mostly seasonal" read: the break is a step on 12 Aug.

Problems, framed as user needs, and what the data shows:
- **1. Slower responder losing the race.** Missed pings 2% to 14% for
  twelve responders, 59% for the four hardest hit; declines flat.
  Confirmed. No response-time column, so "slower" is inferred.
- **2. Responder gone quiet** (Farlight, The Undertow, Meteor Mite,
  Vesper). Their pings fell about 75% (about 12 to 2–4 a week). Tickets
  about it: 0 before, 30 after. Confirmed.
- **3. Handler can't explain a quiet responder.** Mostly repeat
  "is she still on the list?" tickets. Supported, not testable in data.
- **4. Busy responder overloaded.** Top four responders' ping share 32%
  to 43%. Confirmed.
- **5. Handler doesn't know a callout is live.** Not testable.
- **6. Wide-geography responder passed over** (the problem 4.2's ranking
  change targeted). Before: callouts were taken by a same-area responder
  100% of the time, so the complaint isn't visible. After: 78%; Harborside,
  Old Town and Uptown callouts reached their home responder only 7–14%.
  Area is a rough proxy (no travel times). No evidence it worked.
- **7. Filters.** 0.3 to 4 tickets a week; 14 of 16 after release.
- **8. PM can't tell if 4.2 worked.** No success measure, no stated reason
  for the timeout cut, no data after 6 Sep.
- **9. Callouts nobody takes** (found in the data, in no ticket or
  interview). Callouts with a taker 94.5% to 88.9%, so unfilled callouts
  doubled from 5.5% to 11.1%; callouts needing 3+ pings 1.4% to 5.9%.
  Cause not yet checked.

What 4.2 deliberately addressed: only 6 and 7. Problems 1, 2 and 4 are
side effects of the timeout cut (90s to 60s) and the ranking change.
Likely mechanism, unconfirmed: misses count as declines in `history.py`
(-0.12 each, +0.08 per accept), so early misses push a responder down the
ranking and they get few pings. Needs the per-responder recent-acceptance
scores from engineering to confirm.

Caveats: about 56 post-4.2 tickets fit none of my themes and also rose
(5.8 to 16 a week), unexplained. The timeout cut, ranking change and
filter persistence shipped together, so their effects can't be separated
in this data.

**Added at the end of Module 1**
- Names (wiki Team directory, updated 2 Sep): Marcus Oyelaran, engineering
  manager; Wen Li, staff engineer (away 14–24 Aug, right after 4.2); Nadia
  Hoffmann, support lead; Helen Achebe, Director of Product; Sofia Marino,
  designer; Ravi Menon, data analyst (weekly acceptance numbers). Priya
  Raghunathan left 21 Aug.
- The four hardest-hit responders each missed 4–5 of 7–9 pings on 12–16 Aug,
  more than anyone else. The Glossary does describe ranking at a high level;
  what's missing is the weights and numbers.
- My confidence in the story is about 60 out of 95: what happened is solid,
  why it happened is not. Next steps: replay the ping history through
  `history.py`'s scoring, and find what the unfilled callouts share.
- Still to ask: per-responder recent-acceptance scores (Marcus or Wen Li),
  why the timeout was cut to 60 seconds, Ravi's reports and data after
  6 Sep, and last August's numbers to test seasonality.

**Added at the end of Module 2**
- The four interviews (wiki Customer interviews, run by Sofia Marino 2–5 Sep
  for the console redesign) came three weeks after 4.2, so they say nothing
  about why 4.2 was chosen. The wiki has no pre-4.2 research; the Q3 roadmap
  entries for 4.2 are one line each, and nothing explains the timeout cut.
- Weekly acceptance was flat at 75–78% for six weeks, then 54% in the week
  of 10 Aug, and has crept back to 72.7% by 31 Aug. Overall 76.6% to 64.0%;
  missed pings 2.3% to 18.0%, turned-down pings barely moved (21% to 18%).
- Tickets: 40 before 12 Aug, 107 after; 83 open, including all 45 "gone
  before I could answer" (15) and "gone quiet" (30). The quiet tickets are
  about Farlight, The Undertow, Corporal Ashgrove and Halfmoon. Vesper and
  Meteor Mite, two of the four hardest hit by pings, filed no tickets (Dot
  and Kip only came up in interviews). Duplicate-alert tickets went 6 to 0,
  so 4.2's duplicate-push fix seems to have worked. Many tickets repeat
  word for word, so treat ticket counts loosely.
- Availability Confidence was committed to 4.2 on the Q3 roadmap and did not
  ship. Marcus's 14 Aug question (was the ranking change meant to apply to
  responders who turn jobs down?) has no recorded answer.
- Interviews vs tickets: they agree on vanishing and quiet; interviews alone
  over-weighted "callout is live" signal, dark mode and text size, which are
  old requests. Quiet outnumbers vanishing 2 to 1 in tickets. Likely link,
  unconfirmed: responders who miss pings sink in the ranking and then go quiet.
