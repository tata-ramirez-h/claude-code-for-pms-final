# 06 · Sidekicks — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent five sessions on it: finding out what
went wrong, then building the fix nobody had — a working prototype,
by hand, in one sitting, for one room.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Make a skill called review-checklist. When I point it at a brief, it checks that the brief: names who owns it, says how we'll know it worked, keeps the same scope start to finish, and explains the problem before the fix.

### 2.

yes apply these fixes. If there are not more names in the repo, tell me the name of the team, and the seniority of each person or if we have any information and I will give you information

### 3.

okay but you can use the roles in there so you can fix number 1 right? is there anything missing?

### 4.

what success targets are good to choose? can you help me set some parameters?

### 5.

Schedule review-checklist to run every Monday morning, and let me know what it finds. Nothing needs to be ready for it to fire today. I'm setting the habit, not waiting on the result.

### 6.

I updated my protoype, can you include the updated one?

### 7.

please fix this dashboard as the numbers are getting off the box

### 8.

The prototype needs to be in pause, and people needs to clikc play to start everything. It cannot just start playing by itself.

Remove this: "Rook Industries is a fictional teaching scenario from Product School's "Claude Code for PMs" course. Data is from the course database; "before" is 29 Jun–11 Aug, "after" is 12 Aug–6 Sep."

Can you make the top menu work? link it with the actual sections.

Please remove the random space. The graphics in "what happened section are weird", if you do not have the full information, do not add it.

The section "how I found it" please do not divide it by modules, but by findings. And summarize it more, it's too long.

This section is very dense "Keeping it from happening again", it's not easy to read, I just saw it and did not even want to read it. too much text.

Remove the section "next"

### 9.

Perfect, now just remove the following:

* The game is an illustration, not Rook's real ranking. The numbers on the clear and warp screens come from the callout history, and the wording is proposed, not live. We do not know yet if the phone can show a message.
* I have not re-scored the reworked brief, and the folder the schedule reads does not exist yet.
* What the data shows is solid. The cause is a working hypothesis from reading the routing code, and I have labeled each link.
* We do not know yet if the phone can show a message.


Besides removing that, 2 more things:

* Below this text, in the section why it happed, "The timeout started it. The scoring rule made it stick." can you add one sentence summarizing the goal of the section or the main inisight
* In the section "Keeping it from happening again" can you explain how the skill will stop it from happening again? and also include that we have schedule it to run every Monday
