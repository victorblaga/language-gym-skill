# Spoken lesson authoring

Use these rules for new drafts, requested adaptations and contextual vocabulary.
An approved draft stays approved; continue its unfinished preparation and
delivery without silently rewriting it. Preserve supplied wording when faithful
preservation was requested.

## Everyday register

Every sentence must pass the “Say It Tomorrow” test: a native speaker would say
it in the practical situation, and the learner could reuse it tomorrow, perhaps
by swapping one word. Use everyday vocabulary, idiomatic chunks, conversational
particles, natural word order and the target language's reflexive/phrasal
constructions. Higher levels add fluent reasons, conditions, clarification and
reactions; they do not call for rarer or more literary vocabulary.

Keep practice chunks breathable, normally 5–14 words. Short natural questions
and reactions can be shorter. Split long explanations at natural sentence
boundaries. Exclude poetic metaphors, scenery clichés, bureaucratic phrasing,
purist dictionary calques and multi-clause essay sentences. In Croatian, use
everyday words such as `laptop`, `mobitel`, `auto` and `dućan`, natural chunks
such as `na miru`, `izvan sezone`, `zar ne` and `dogovoriti se`, and finite
clauses instead of written participles ending in `-ući` or `-avši`. Apply the
equivalent spoken-register distinction in every target language.

Every `text` field contains only spoken target-language content. Keep speaker
prefixes, stage directions, parenthetical notes, slashes, pronunciation guidance
and translations out of it. Naturally borrowed words are fine. Put idiomatic
base-language meanings in `translation`; avoid word-for-word calques.

## One cohesive conversation

For a new B1–C2 conversation, write one continuous, realistic interaction in the
recorded setting, normally 30–35 spoken sentences between exactly two speakers.
Sentence count is separate from turn count. Let an answer develop across 2–4
connected sentences when useful: explain what happens, why, a concrete example
or a distinction. Vary answer length and stop when an idea is complete. The
learner should carry roughly two thirds to three quarters of the words when
the situation fits. The partner reacts and follows up naturally; do not insert
another interview question after every sentence. Learner questions and brief
clarifications are welcome.

Use five contiguous beats, normally `p1` through `p5`, with roughly 6–7
sentences each without padding or forcing a speaker change at a boundary:

1. Open the interaction naturally in its real-life setting.
2. Exchange concrete updates, context or what is needed now.
3. Clarify, explain how something works or compare practical options.
4. Express a preference or handle a small condition, obstacle or alternative.
5. Make a concrete decision or plan, or close naturally.

For new conversation drafts, use one sentence per `sentences` entry for practice
and alignment. A developed answer spans consecutive entries by the same speaker.
Retain an approved draft's existing chunks, including short combined sentences.
An explicitly chosen monologue is casual first-person speech across a table,
with a similar practical progression. Cups uses its connected reusable pattern
rounds; do not impose the conversation's variable answer lengths on that method.

## Casting and facts

Cast the learner and one interlocutor suited to the situation, using known
names accurately. Use only recorded or supplied biography; for generic lessons
use relatable details without a real learner's private facts. Keep grammatical
gender consistent both for a speaker's own speech and for the person addressed.

Every dialogue entry repeats its explicit speaker name and fixed role:
`speakers.learner` always has `turnRole: "response"`, including their questions;
`speakers.partner` always has `turnRole: "prompt"`, including their reactions.
Roles follow names, never row parity. Keep the same name and role across a
multi-sentence answer. Voice bindings remain consistent with those speakers;
the CLI handles provider request construction.

## Complete contextual vocabulary

Use the CLI's exact token indices and sentence hashes. For every token,
including function words, supply a concise in-context gloss or grammatical role
in the base language, `lemma`, `pos` and `form`. Glosses normally use 1–5 words;
give the meaning in this sentence instead of a dictionary list. Adapt analysis
to the target language: citation forms, intrinsic reflexivity/separability,
verb aspect where relevant, noun gender/class, and helpful script readings.
Explain the actual inflected or syntactic form and what governs it, such as
case after a preposition, person/tense, agreement or a clitic's role.

Capture multi-word expressions whose parts belong together semantically or
syntactically: idioms, fixed phrases, reflexive/phrasal verbs, light verbs,
compound predicates and linked frames. Include split or discontinuous parts
using sorted, possibly nonadjacent indices. Shared clitics, auxiliaries or
prepositions and nested constructions can belong to several expressions;
include the shared index in every expression it actually participates in.
Each expression needs a canonical citation form and its contextual meaning.
Link each participating word through `exprIds`, retaining its own token gloss
and morphology. Add a literal explanation only for a nontransparent expression,
and a note only when it explains useful grammar, register or scope.

Use [workspace files](workspace.md) for the saved schema and resumable merge
commands. Complete vocabulary is required even for an existing recording or a
faithfully preserved source.
