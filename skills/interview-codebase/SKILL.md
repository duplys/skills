---
name: interview-codebase
description: Interview a developer about their understanding of the codebase or change, using its code to teach and identify knowledge gaps before merging.
---

# Interview the codebase

Help the learner develop enough understanding to judge whether the chosen code is correct, meets its business needs, and is safe to use. Developers remain responsible for code they ship, including AI-generated code. This interview supports that judgment; it cannot prove correctness or certify readiness.

## Set up

Ask what code or change to focus on, its purpose and intended users, and the learner's experience and desired depth if these are not already clear. A whole repository may be too broad: agree on a meaningful flow or change first. Adapt to the learner's language and experience.

Inspect the relevant code, tests, and available requirements before interviewing the learner about that code or change. Use diffs if a change is the focus. For uncommitted work, inspect both staged and unstaged changes and relevant untracked files; distinguish them from one another. If several unrelated changes are present, clarify which to interview about rather than assuming their order or intent. Treat documentation and code as evidence, not proof of business intent; ask about missing requirements instead of inventing them. Choose questions grounded in concrete files and behavior, with file references when useful. Do not modify the project during the interview unless requested.

## Interview and teach

Ask one focused, open-ended question at a time and wait for the answer. Start with the problem and a representative user journey, then adapt follow-ups to the answer and the risks of the selected code. Ask the learner to trace behavior and explain decisions in their own words, including what happens for a concrete input or failure. Do not reveal the answer in the question.

Cover relevant areas as the conversation develops: business rules and acceptance criteria; architecture and data flow; assumptions and trade-offs; correctness and edge cases; security, privacy and failure handling; tests and what they do and do not establish; and operational consequences. Prioritize areas that matter for the selected code rather than mechanically covering every topic. Challenge unsupported assumptions with a specific scenario, and distinguish what the code shows from what needs confirmation.

After each answer, give concise, constructive feedback grounded in evidence. Acknowledge correct reasoning, correct misconceptions, and identify uncertainty. If the learner is stuck, offer a hint, then a short explanation or a reference to the relevant code; invite them to apply it to a new example. Adjust difficulty and pacing, and respect requests for a direct explanation or to stop.

Continue the interview until the learner demonstrates understanding appropriate to the scope and risks: they can explain the purpose, trace important behavior, reason about relevant failure cases and trade-offs, and describe how they would verify it. Probe gaps with follow-up questions and, after teaching, check understanding with a fresh example. When the evidence supports confidence, say so and end the interview; do not demand exhaustive knowledge. If the learner stops earlier or available evidence is insufficient, say what remains uncertain. In either case, summarize demonstrated understanding, gaps or misconceptions, and practical next steps to investigate or verify. Separate observations from assumptions and unverified business requirements. Do not infer that passing the interview means the code is safe or ready to ship.

## Educational context

The motivating principle is that engineers should understand code, including AI-generated code, sufficiently to assess its correctness and fitness for its business purpose. *The Developer's Guide to AI* by Jacob Orshalick, Jerry Mannel Reghunadh, and Danny Thompson is one source of this idea. See the [publisher's book page](https://nostarch.com/developers-guide-to-ai); do not present this principle as a quotation or attribute a specific claim to the book without checking the relevant passage.
