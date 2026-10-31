# Brief: Four responders went quiet after 4.2. What do their handlers and they see?

For Helen Achebe. Rough draft. Nothing is built or shipped, and nothing ships until you have seen this.

**Owner:** the Product Manager, Rook Dispatch. Owns this proposal and, with Helen Achebe, the decision to go ahead, stop or roll back. [who calls the roll-back: name or role]

**Scope:** the four responders who went quiet (Farlight, The Undertow, Meteor Mite, Vesper). Meteor Mite and their handler Kip are the worked example throughout.

## 1. Executive overview
Since 4.2, four responders get 3 to 4 pings a week instead of about 12 (Meteor Mite: 1 to 4 instead of about 11), and their handlers have nothing true to tell them. I propose one change with two parts: tell the handlers and responders what is happening, and give the four a real way back into the pings by changing how a miss is scored.

## 2. The problem and why now
Four of our 16 responders went quiet after 4.2 shipped on 12 Aug. They now miss 53 to 64% of their pings (1 to 4% before), and handlers have no explanation to give. Tickets about quiet responders went from 0 before 12 Aug to 30 after, and they keep coming even as the overall acceptance rate recovers (72.7% in the week of 31 Aug), so waiting will not fix it. The people this happens to are the responder, who is passed over without being told why, and the handler, who is asked "is she still on the list?" and has only "hang in there" to say. Detail and caveats are in section 6.

## 3. Decisions we need from Helen

| Decision | My suggestion |
|---|---|
| Do we go ahead with this proposal as a plan to take to engineering? | Yes |
| Try it on the four quiet responders first, once engineering says it is safe? | Yes, with the stop rule below |
| Does this replace a Q3 commitment? | Please tell me |

## 4. Decisions we need from other teams
- **Engineering (Marcus Oyelaran, Wen Li):** Is the recovery rule safe, and what size should it be? Can the phone show a message?
- **Rook Supply:** Does the change touch the availability record they read? I do not know yet. Not asked.
- **Design (Sofia Marino):** Do the words fit the screens?

## 5. Ideal state and plan

Proposed wording below, not live. The same words would go to the other three responders and their handlers.

**What Kip sees** on Meteor Mite's card: "Quiet since 12 Aug. Pings a week: 11 before, 1 now. The ping wait was cut from 90 to 60 seconds that day. We are checking if that is why. Meteor Mite is still on the list. Misses now count less against ranking."

**What Meteor Mite sees** on their phone: "We see you are getting fewer pings since 12 Aug. We are checking why. You are still on the list. Missed pings now count less against your place, and it recovers over time."

**The recovery rule (the part that changes `history.py`):** today a miss counts like a "no" (-0.12), only a yes adds points (+0.08), and nothing ever fades. My starting idea is that a miss costs half as much, and scores drift back toward neutral over time. The size and the time are for Wen Li and Marcus to set. Wen's old question, whether a miss should count like a "no" forever, is the reason for this part. My view is no, because a miss is not a choice.

**Who may lose pings:** The Gale and the busiest responders. The Gale went from about 13 pings a week to 17 to 21 after absorbing Meteor Mite's share.

**Plan:** Marcus confirms nothing changed on the four accounts. Engineering sets the rule. Sofia checks the screens. We try it on the four, with the other 12 as comparison, and check after four weeks.

**Success** (measured on the four quiet responders; Meteor Mite shown as the example):

The trial runs four weeks from the day the rule goes live [start date: set when engineering says it is safe]. Targets are my proposals, to be checked with Ravi Menon (data) and Wen Li (drift speed). Judge on four-week totals, because the four together get only about 14 pings a week, and compare against the other 12 responders over the same weeks.

| Measure | Before 4.2 | Today | Target (week 4) | Stretch |
|---|---|---|---|---|
| Pings a week, each of the four | about 12 | 3 to 4 | 8 or more | 12 |
| Meteor Mite pings a week | about 11 | 1 | 8 or more | 11 |
| Share of their pings missed | 1 to 4% | 53 to 64% | 25% or less | 18% or less |
| "Gone quiet" tickets a week | 0 | about 7 (30 since 12 Aug; check with Nadia Hoffmann) | 2 or fewer, judged on weeks 3 and 4 | 0 to 1 |

If the share missed is still above 40% at week 4, the rule is not working or the cause is something else (for example a phone or alert problem), and we rethink.

**Must not get worse than today:**

| Measure | Today | Guardrail |
|---|---|---|
| The Gale pings a week | 17 to 21 | no higher than 21, ideally back to 17 or less |
| Overall acceptance | 64.0% (72.7% latest week) | no lower than 64.0%, aiming to hold 72.7% |
| Callouts nobody took | 11.1% | 11.1% is the stop line, aiming for 8% or less |

**Stop rule:** roll back if callouts nobody took are above 11.1% in two weeks in a row. That rate also rose in other areas after 4.2, so one bad week alone does not count. [who calls the roll-back: name or role]

## 6. Supporting data
Before is 29 Jun to 11 Aug. After is 12 Aug to 6 Sep.

| | Before | After |
|---|---|---|
| Meteor Mite pings a week | 10 to 12 | 7 (part week), 4, 2, 1 |
| Eastgate pings, Meteor Mite / The Gale | 62 / 76 | 9 / 54 |
| Four quiet responders, pings missed | 1 to 4% | 53 to 64% |
| Callouts nobody took | 5.5% | 11.1% |

Kip (4 Sep interview): Meteor Mite keeps asking "if something's broken", and Kip has only "hang in there." The cause is not proven. The 60 second wait likely started the misses, and the code never forgives a miss. If the real cause is a phone or alert problem, the words above must change.

## 7. Open questions, with owners

| Question | Owner |
|---|---|
| Ranking scores over time? Why was the wait cut to 60 seconds? | Marcus Oyelaran |
| What size and time for the recovery rule? | Wen Li and Marcus Oyelaran |
| Did the ranking change apply to responders who turn jobs down? (asked 14 Aug) | Wen Li |
| Anything change on Meteor Mite's account or phone around 12 Aug? Can the phone show a message? | Marcus Oyelaran |
| Numbers after 6 Sep and last August? | Ravi Menon |
| Meteor Mite's own miss rate, and their own words (not asked yet)? | Product Manager, Rook Dispatch |
