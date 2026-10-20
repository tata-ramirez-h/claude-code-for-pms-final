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

**Added after the per-responder ping analysis**
- Saved data in `00-rook/data/`: `callout-history.csv` (responder x week,
  pings and taken, 175 rows), `weekly-totals.csv`, `daily-totals-10-31-aug.csv`.
  All come from the `pings` table. The 10 Aug week mixes before and after 4.2
  (release was Wed 12 Aug), so it is split in the responder file.
- Daily: acceptance was 68-75% on 10-11 Aug, then 40-53% on 12-16 Aug, with 4-12
  missed pings a day (0-1 before). Weekly acceptance by week from 29 Jun: 75-78%
  for six weeks, then 54.2, 65.8, 66.7, 72.7. Missed pings per week: 2-6, then 38,
  28, 24, 21. Turned-down pings barely moved. Straight trend lines understate
  the break.
- Tickets lag pings: 5-8 a week, then 20, 27, 32 (peak, week of 24 Aug), 25.
  Missed pings peaked a week earlier, so quiet-responder complaints keep
  coming while the overall rate recovers.
- Per responder: Farlight, The Undertow, Meteor Mite and Vesper fell from about
  12 pings a week to 3-4. They now miss 53-64% of pings (1-4% before); the other
  12 miss 10-18% (0-5% before). Ticket counts do not track the size of the drop:
  Meteor Mite and Vesper filed none, Halfmoon and Corporal Ashgrove (about
  25-30% fewer pings) filed 12 and 10. Interviews match the data: Kip's
  Meteor Mite quiet and The Gale busy (The Gale pings up about 60%), Dot's
  Vesper quiet stretches.
- Ruled out in the data: the quiet four are not pinged late in the chain (71%
  of their pings are still first position, like everyone else) and their misses
  are not a night-hours effect (50-77% missed at every hour). They are reached
  far less often, and when pinged first, 70% of the time they miss (17% for the
  other 12). Only 56 pings after 4.2, so time-of-day splits are tiny.
- Still open: why the four stopped answering (disengaged after early misses,
  device or notification changes, or something else), and the ranking
  mechanism. Rough check with history.py weights (-0.12 miss, +0.08 accept):
  all four net negative in their first five days after 4.2. Not a replay.
- To ask Marcus or Wen Li: did anything change on those four accounts or
  devices around 12 Aug, and can they pull per-responder ranking scores over
  time? Not testable here: slower response time (no column), seasonality (no
  last August data).

**Added after the home-base and who-took-it analysis**
- Every responder has a home area (`responders.area`). Before 4.2, no ping
  to a responder outside their home area was ever taken (0 of the out-of-area
  pings). After 4.2, nine responders took 86 callouts outside home, all in
  three areas: Harborside, Old Town and Uptown. In every other area outsiders
  took none, before or after.
- Those three areas belong to The Undertow, Vesper and Farlight, three of the
  four who went quiet. In them, the first ping went to the home responder 75-85%
  of the time before and 13-25% after. The home responder took 92-97% of
  callouts before and 6-13% after. Outsiders who took them: Stormwrack 14,
  Ironvale 9, The Drift 4 (Harborside); Captain Vantage 14, Nightwell 13, The
  Longcast 4 (Old Town); Sgt. Falkirk 14, Cindermark 9, Sgt. Bulwark 5 (Uptown).
- Eastgate has two responders, The Gale and Meteor Mite (same handler, Kip).
  Meteor Mite's Eastgate pings fell from 62 to 9 and The Gale's went from 76 to
  54: The Gale absorbed Meteor Mite's share. The Gale also gets about 4 pings a
  week outside Eastgate (Midtown, Foundry Row), none taken.
- Callouts with nobody taking them rose from 5.4% to 13.0% in the other areas
  too, so that rise is not only in the three areas. Area is a rough proxy (no
  travel times) and "first pinged" is inferred from send order; the ranking
  scores are still not visible. Still open: why first pings left the home
  responders in those three areas.
- Saved: `00-rook/data/pings-by-responder-and-area.csv`,
  `first-ping-and-taker-by-area.csv`, and `sankey-who-took-callouts.html`
  (open in a browser).

**Added after reading the routing code against the home-base data**
- Location did not stop mattering. 4.2 raised the proximity weight (0.45 to
  0.60) and cut recent acceptance (0.40 to 0.25), so history can now swing a
  ranking by at most 0.25 points, not 0.40. In 12 of 15 areas outsiders took
  no callouts, before or after. Only Harborside, Old Town and Uptown (the home
  areas of The Undertow, Vesper and Farlight) stopped following geography. My
  earlier charts showed only those three, which overstated it.
- So the ranking-weight change probably is not the cause; it should favor home
  responders. Working hypothesis, unconfirmed: the timeout cut (90s to 60s)
  started the misses, and `history.py` made them stick (a miss counts as a
  decline, -0.12, floor 0, no decay; a 2019 TODO notes it never recovers on its
  own). Outsiders can outrank a home responder only if their travel time is
  within about 19 minutes of the home responder's (my arithmetic from the
  weights; travel times are not in the data). Check whether those three areas
  border the outsiders' areas.
- Not explained by the code: outsiders pinged for those three areas took 0 of 44
  before 4.2 and 86 of 133 (65%) after, while outsiders pinged for other areas
  still took none. They started saying yes.
- Problem 6 (wide-area responder passed over, the target of the ranking
  change): no evidence it worked, and in three areas the home responders kept
  only 6-13% of their own area's callouts. Code read is the repo's version, which
  may differ from production. To ask Wen Li or Marcus: per-responder ranking
  scores and travel times, and whether anything changed on those three
  responders around 12 Aug.

**Added at the end of Module 3**
- Headline for Helen Achebe: 18% of pings are now missed, up from 2% (110 of 611 vs
  25 of 1,085; overall acceptance 76.6% to 64.0%). The break is on 12 Aug, not a
  slow seasonal slide, and turned-down pings barely moved.
- The story is who it hit, not the average: four responders (Farlight, The
  Undertow, Meteor Mite, Vesper) went quiet, The Gale and a few others got
  busier, and in Harborside, Old Town and Uptown outsiders now take most callouts.
- Reading the routing code, the timeout cut probably started the misses and the
  scoring rule made them stick; 4.2's ranking change alone should favor home
  responders. Confirm with per-responder ranking scores and travel times.
- Weekly acceptance recovering (72.7%) hides that the quiet four are still near
  zero and tickets keep arriving, so don't report recovery on the headline alone.
- Still to ask: Marcus or Wen Li (scores, travel times, anything that changed on
  those accounts around 12 Aug), whether anyone answered Desmond Okafor's open
  tickets, and last August's numbers.

**Added at the end of Module 4**
- Reading `00-rook/code/dispatch-routing/` end to end: only `history.record_accepted` (+0.08, called from `offer.py` on a yes) ever adds points; `record_declined` (-0.12) fires for a decline and a miss alike. No decay, reset or manual boost, so a responder needs about 60% yes just to hold steady. Scores are an in-memory dictionary keyed by name in this repo version; production may differ. `availability.py` and the phone functions in `offer.py` are empty stubs, so travel time, pushes and answer detection are unseen.
- The quiet four are not unpinged: they still get 3-4 pings a week and say yes only about 18-29% of the time. Per-day pings fell 66-75% for them. Out-of-area pings to them: 50 before, 22 after, 0 taken, so zones outside home won't pull them out.
- Marcus's 14 Aug Slack question (did the ranking change apply to responders already turning jobs down?): in the code it applies to everyone and softens the history penalty. In the data, heavy decliners before 4.2 (13-29%) mostly kept or gained pings; the responders who lost them are the ones whose misses jumped. A reply is drafted but not sent. Intent is still unrecorded.
- Rook Supply reads the availability record, and its shape is a contract: ask the Supply team before any availability change, including Availability Confidence. Supply's side is unseen.
- Engineering levers if asked: count misses differently, restore 90s, raise the score floor, let scores drift to neutral (2019 TODO, a considered choice by an unnamed author), reset the four. Open: why the four miss (response time, device or notification), per-responder scores, why the timeout was cut.
