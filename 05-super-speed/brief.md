# Brief: Meteor Mite went quiet. What do Kip and Meteor Mite see?

For Helen Achebe. Rough draft. Nothing is built or shipped, and nothing ships until you have seen this.

## 1. Executive overview
Since 4.2, Meteor Mite gets 1 to 4 pings a week instead of about 11, and their handler Kip has nothing true to tell them. I recommend we first fix what Kip and Meteor Mite *see*, then decide with you whether to change how misses are scored.

## 2. Decisions we need from Helen

| Decision | My suggestion |
|---|---|
| Start with Option 1 (words only)? | Yes |
| Try Option 2 (scoring change) on the four quiet responders, once engineering says it is safe? | Yes, with the stop rule below |
| Does this replace a Q3 commitment? | Please tell me |

## 3. Decisions we need from other teams
- **Engineering (Marcus Oyelaran, Wen Li):** Is Option 2 safe? Can the phone show a message?
- **Rook Supply:** Does Option 2 change the availability record they read? I do not know yet. Not asked.
- **Design (Sofia Marino):** Do the words fit the screens?

## 4. The problem and why now
Four of our 16 responders, Meteor Mite among them, went quiet after 4.2 shipped on 12 Aug. Tickets about them keep coming even as the overall rate recovers, so waiting will not fix it.

## 5. Ideal state and plan

| | Option 1: words only | Option 2: Option 1 plus scoring change |
|---|---|---|
| Kip's card | "Quiet since 12 Aug. Pings a week: 11 before, 1 now. The ping wait was cut from 90 to 60 seconds that day. We are checking if that is why. Meteor Mite is still on the list." | Adds: "Misses now count less against ranking." |
| Meteor Mite's phone | "We see you are getting fewer pings since 12 Aug. We are checking why. You are still on the list." | Adds: "Misses in your first weeks back will not hurt your place." |
| Code change | Text only | Also changes how a miss is scored |
| Who may lose pings | No one | The Gale and the busiest responders |

**Wen's old question:** should a miss count like a "no" forever? My view is no, since a miss is not a choice. That needs Wen's and Marcus's sign-off.

**Plan:** Marcus confirms nothing changed on the four accounts. Ship Option 1. Try Option 2 on the four, with the other 12 as comparison. Check after four weeks.

**Success:** More pings and takes for Meteor Mite, fewer "is she still on the list?" tickets. The Gale's load, overall acceptance and unfilled callouts must not get worse. **Stop rule:** roll back if unfilled callouts go above 11.1% (today's level).

## 6. Supporting data
Before is 29 Jun to 11 Aug. After is 12 Aug to 6 Sep.

| | Before | After |
|---|---|---|
| Meteor Mite pings a week | 10 to 12 | 7 (part week), 4, 2, 1 |
| Eastgate pings, Meteor Mite / The Gale | 62 / 76 | 9 / 54 |
| Four quiet responders, pings missed | 1 to 4% | 53 to 64% |
| Callouts nobody took | 5.5% | 11.1% |

Kip (4 Sep interview): Meteor Mite keeps asking "if something's broken", and Kip has only "hang in there." The cause is not proven. The 60 second wait likely started the misses, and the code never forgives a miss. If the real cause is a phone or alert problem, the wording above must change.

## 7. Open questions, with owners

| Question | Owner |
|---|---|
| Ranking scores over time? Why was the wait cut to 60 seconds? | Marcus Oyelaran |
| Did the ranking change apply to responders who turn jobs down? (asked 14 Aug) | Wen Li |
| Anything change on Meteor Mite's account or phone around 12 Aug? Can the phone show a message? | Marcus Oyelaran |
| Numbers after 6 Sep and last August? | Ravi Menon |
| Meteor Mite's own miss rate, and their own words (not asked yet)? | Me |
