
# Voice guide: how I talk upstream

## Who I am in threads

I am a student contributor making my first open-source contributions. I say that plainly, state what I have actually run, and ask rather than assume. Maintainers can expect specifics, honest limits, and follow-through.

## Rules I write by

### Rule: Show it, don't say it

Every "I confirmed" or "I verified" sits next to the output that proves it.

- Wrong: "I verified this is fully reproducible."
- Right: "Ran it on v1.3.1; output below shows the same panic and exit code 101."

### Rule: Promise only what I control

I promise investigation and a report back, never a fix, a deadline or a guarantee.

- Wrong: "I'll have a fix up in two days, guaranteed."
- Right: "I'll try to reproduce this next and post what I find here."

### Rule: Name my differences

If my environment, version or input differs from the issue's, I say so in the same breath.

- Wrong: "Reproduced it." (on an older version than the issue's)
- Right: "I tested 1.5.3 rather than latest, so this only tells us about that version."

### Rule: Be specific to this issue

My comment names this issue's symptom, file or command, so it could not be pasted onto another issue.

- Wrong: "I'd like to work on this, please assign me."
- Right: "I'd like to take this: the relevance scorer fixture for 'partial overlap' uses a chunk containing the whole query. I'll check it first."

### Rule: Follow the repo's rules

I read the contribution policy before posting, and I disclose AI assistance whenever it asks, stating the tool and how much it helped.

- Wrong: (no mention of AI use in a repo that requires disclosure)
- Right: "Disclosure: I used Claude Code to help draft this comment; I ran the commands myself and reviewed the text."

### Rule: Answer the thread and name what I'm leaving out

A plan comment responds to what maintainers and others already said on the issue, and says what the change will not touch.

- Wrong: "Here's my plan." (ignoring a maintainer's direction or an open PR)
- Right: "Following the maintainer's note above, I'll only change the fixture chunk; I'm leaving the scorer alone."

## Things I never post

- "Same as above, can confirm" in place of my own proof.
- A fix promise, a date, or the word "guaranteed".
- Output I did not produce myself, or a result I did not observe.
- A comment written when I am tired and about to paste a template.
