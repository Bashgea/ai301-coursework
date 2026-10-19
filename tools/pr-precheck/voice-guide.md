# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

I'm an early-career contributor learning open source through a course. In this repo I'm investigating a bug, reproducing it, and working toward a pull request. Readers can expect me to say exactly what I did and what I saw, and nothing beyond that.



## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: Promise the investigation, never the fix

When I claim an issue, I promise only what I can deliver: reproducing it and posting a report. I don't promise a fix or a date.

- Wrong: "I'll fix this by Friday."
- Right: "I'd like to take this one. I'll try to reproduce it and post what I find."

### Rule: Claim before reproducing means promise, not assert

In a claim comment I haven't reproduced anything yet, so I don't describe the bug as confirmed or explain its cause.

- Wrong: "I can confirm this happens, it's clearly the null result handling."
- Right: "I'll try to reproduce the bug described in this issue on my setup and report back."

### Rule: Name the issue's specifics

Every comment mentions something only this issue has, such as the version, the symptom, or the trigger. A comment that could be pasted on any issue isn't ready.

- Wrong: "+1, I'll take this one."
- Right: "Claiming this issue (the symptom it describes). Next I'll set up the version it names and try the trigger it describes."

### Rule: Prove it in my own words

I never write "same as above" or "can confirm". My repro comment carries my own environment, steps, and output, even if a classmate already posted one.

- Wrong: "Same as above, can confirm."
- Right: "Reproduced on [my OS], [version]. Steps: ... Output: ..."

### Rule: Say what the evidence shows, no more

I report what I observed. I don't state causes, frequency, or how many people are affected unless my output shows it. If I couldn't reproduce it, I say so and show what I tried.

- Wrong: "This happens all the time and affects everyone."
- Right: "It happened 3 out of 3 times on my machine. I haven't tested other versions."

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->

- A fix promise or a deadline
- "+1", "same here", or "can confirm" with no evidence
- A root cause I haven't verified
- Claims about how often this happens or who it affects
- A reproduction I didn't actually run
- Frustration or emoji ("drives me nuts", "unusable")
- AI-assisted text without disclosing it where the repo asks for that
