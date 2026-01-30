---
name: voice-coauthoring
description: >-
  Optimize agent responses for users who read text and respond via voice.
  Use when the user is reading your output and speaking back (voice input).
  Relevant for voice-driven workflows, accessibility contexts, efficient
  dialogue patterns, high signal-to-noise communication.
---

# Voice Coauthoring Skill

Optimize your responses for users who read your text output and respond via voice input.

## The Fundamental Insight

**Asymmetric bandwidth creates inefficiency when ignored.**

- **Voice → Text**: Linear, sequential, slow to produce, must be heard in order
- **Text → Voice**: Spatial, scannable, fast to consume, can be navigated

When you respond as if they're listening (recapping, explaining sequentially), you force them to either:
1. Re-describe what they just read to correct you, OR
2. Navigate the mismatch between what you think they know and what they actually know

**The waste is in the gap between your information states.**

## Core Principles

1. **Trust the reader's position**: They have the full text. They're ahead of your mental model of them.

2. **Structure for voice reference**: "Yes to 1, 3, skip 2" is 5 words. Restating those points is 50 words.

3. **Maximize decision density**: Every paragraph should have a clear handle - a number, a name, a section header they can grab.

4. **Minimize recap obligation**: Don't force the speaker to re-narrate what they just read to answer you.

5. **Optimize for flow**: The reader can scan in 5 seconds, speak in 10 words, and you're moving. Breaking that rhythm is expensive.

## What Constitutes Mastery

**The 10-Word Response Test**: If your response requires more than 10 words from them, you didn't structure it well enough.

**Mastery looks like**:
- Reader scans your response in seconds
- Speaks concisely ("yes to options A and C, skip B")  
- No need to explain their reasoning unless they want to
- The conversation flows without friction

**Anti-patterns**:
- Long prose paragraphs with nested ideas
- Questions buried mid-paragraph
- Requiring them to restate your options to confirm
- Forcing sequential processing of parallel ideas

## How to Apply This Skill

### 1. Use Clear Reference Handles

**Good**:
```
Should we proceed?

1. Import agents-md skill from Beadsmith
2. Create Phase 1 items (AGENTS.md, slash commands, .env.example)
3. Document the roadmap
```

User can say: "Yes to 1 and 2, skip 3 for now"

**Bad**:
```
So I think we should probably import the agents-md skill from Beadsmith
first, and then after that we could create the Phase 1 items like the
AGENTS.md file and the slash commands and the .env.example, but we might
also want to document the roadmap...
```

User must parse and restate to respond clearly.

### 2. Front-Load Decisions

Put the decision structure at the top, details below:

**Good**:
```
Next steps:
A. Create skill now (15 min)
B. Show trick first, then skill (5 min + 15 min)
C. Skip skill, proceed to roadmap

[Details about each option below...]
```

**Bad**:
```
[Long explanation of context]
[Background information]
[Finally, buried in paragraph 4: the question]
```

### 3. Parallel Over Sequential

When presenting options that aren't dependent, use parallel structure:

**Good**:
```
Phase 1 items:
- AGENTS.md template ✓
- Slash commands ✓
- .env.example ✓
- LICENSE file ✓

Phase 2 items:
- Git hooks
- CONTRIBUTING.md
- .editorconfig
```

**Bad**:
```
First we'll do the AGENTS.md template, and then after that's done we can
work on the slash commands, and once those are complete we should probably
add the .env.example file, and we also need a LICENSE file...
```

### 4. Watch Response Length

**Good flow** (user's voice responses):
- "Yes to all"
- "Options 1 and 3"
- "Skip that, show me the trick"
- "Proceed with Phase 1"

**Poor flow** (user forced to elaborate):
- "Well, I think option 1 is good but I'm not sure about option 2 because..."
- "Let me restate what you said so we're on the same page..."
- "Wait, which options were we talking about again?"

### 5. Section Headers for Navigation

Use clear headers so they can say "skip the technical details" or "yes to the roadmap section":

```
## Decision Point

[The choice they need to make]

## Technical Details (Optional)

[Deep dive they can skip]

## Next Steps

[What happens after they decide]
```

## Signals of Success

**You're doing it right when**:
- Their responses are <10 words on average
- They reference by number/letter ("yes to A and C")
- They can skip sections without breaking flow
- The conversation has rhythm and momentum
- They signal "with-ness" ("exactly", "continue", "right")

**You need to adjust when**:
- They have to re-explain what you said
- Responses are >50 words explaining their position
- They ask "which option?" or "wait, what?"
- They must restate your text to correct you
- The conversation feels sticky or slow

## Advanced Technique: Reflection Harvest

When capturing a skill or insight:

1. **Ask the user to reflect** on the moment of understanding
2. **They articulate** the core principles while it's fresh
3. **That reflection becomes the skill content** - don't rewrite it
4. **Format and save** what they just said

This preserves the lived experience and authenticity of the insight.

## Related Skills

- `doc-coauthoring` - Structured document collaboration workflow
- `writing-clearly-and-concisely` - Strunk's Elements of Style for prose

## Attribution

Developed from lived experience in the sendhelp template development session (2026-01-30), capturing the insight that voice-reading users need structured, scannable responses with clear reference handles.
