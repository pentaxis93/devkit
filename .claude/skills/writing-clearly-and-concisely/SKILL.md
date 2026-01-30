---
name: writing-clearly-and-concisely
description: >-
  Apply Strunk's timeless writing rules to any prose humans will read —
  documentation, commit messages, error messages, explanations, reports,
  or UI text. Use when writing or editing prose for clarity, conciseness,
  and strength. Relevant for copyediting, style review, verbose drafts,
  passive voice, vague language.
---

# Writing Clearly and Concisely

## Overview

William Strunk Jr.'s *The Elements of Style* (1918) teaches clear writing
and ruthless cutting.

**WARNING:** `references/elements-of-style.md` consumes ~12,000 tokens.
Read it only when actively writing or editing prose.

## When to Use This Skill

Use this skill when writing prose for humans:

- Documentation, README files, technical explanations
- Commit messages, pull request descriptions
- Error messages, UI copy, help text, comments
- Reports, summaries, or any explanation
- Editing to improve clarity

**If writing sentences for a human to read, use this skill.**

## Limited Context Strategy

When context is tight:
1. Write the draft using judgment
2. Dispatch a sub-agent with the draft and `references/elements-of-style.md`
3. Have the sub-agent copyedit and return the revision

## All Rules

### Elementary Rules of Usage (Grammar/Punctuation)
1. Form possessive singular by adding 's
2. Use comma after each term in series except last
3. Enclose parenthetic expressions between commas
4. Comma before conjunction introducing co-ordinate clause
5. Do not join independent clauses by comma
6. Do not break sentences in two
7. Participial phrase at beginning refers to grammatical subject

### Elementary Principles of Composition
8. One paragraph per topic
9. Begin paragraph with topic sentence
10. **Use active voice**
11. **Put statements in positive form**
12. **Use definite, specific, concrete language**
13. **Omit needless words**
14. Avoid succession of loose sentences
15. Express co-ordinate ideas in similar form
16. **Keep related words together**
17. Keep to one tense in summaries
18. **Place emphatic words at end of sentence**

### Section V: Words and Expressions Commonly Misused
Alphabetical reference for usage questions

## Bottom Line

Writing for humans? Read `references/elements-of-style.md` and apply the
rules. Low on tokens? Dispatch a sub-agent to copyedit with the guide.

## Attribution

Adapted from [obra/the-elements-of-style](https://github.com/obra/the-elements-of-style).
Reference text is public domain (William Strunk Jr., 1918, via Project Gutenberg).
