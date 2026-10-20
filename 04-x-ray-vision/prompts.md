# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Can you check if we have this folder: 00-rook/code/dispatch-routing/

### 2.

Can you read the code start to finish. Tell me what is does. Describe the whole thing in plain English, the way you'd explain your own product to somebody outside the room.

### 3.

Can you explain this further? in English terms:

* Rook Supply reads the availability record. Supply uses it to schedule gear servicing when a responder is unlikely to be called out. It only reads. If the shape of that record changes, the Supply team has to be told first.

### 4.

What this code does not do? Code only shows you what somebody built. What is missing?

### 5.

Help me fill this statement "Based on what I found, the reason some responders are getting no pings at all is ___, because ___."

### 6.

Marcus, an engineering manager, asked a question in Slack a week ago. Nobody's answered it.

Marcus Aug 14, 2:47 PM
Quick one — did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?

In order for me to respond this, I need to check the results we found in the last session with number of pings, missed pings, callouts, etc. with the code and the recent insights

### 7.

When you're called on, say the answer to their question out loud, in one sentence, the way you'd say it to them.

### 8.

Somebody has been quiet for a month. Walk me through, step by step, exactly what would have to happen for them to start getting work again.

### 9.

Could taking pings in another zones that are not their home-base zones help?

### 10.

Is there a way in the code to get more pings? because clearly the main problem is they are not getting enough pings to say yes so it's a vicious cycle

### 11.

Help me fill this: "According to my findings in the code, for someone who's gone quiet, they would need to ___."
