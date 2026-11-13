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

## Readings and word boundaries

Use the reading convention and spoken variety recorded for the selected
language. Mandarin uses pinyin automatically, with tone marks, ü, ordinary
word spacing and unmarked neutral tones: `我喜欢喝茶。` →
`Wǒ xǐhuan hē chá.` Do not ask the learner to approve the established standard.
For a meaningful first-language setup choice, explain what changes in practice,
recommend one option and ask one question at a time. For example, Arabic needs
a spoken variety and a consistent named learner-friendly convention; `ar` and
unvowelled script do not establish either. Reuse recorded choices on later
lessons. A display preference does not change the authored convention.

Create readings with the rest of the lesson using your own language capability.
Every sentence, lookup word, expression and displayed constituent needs a
reading when the language is configured to use one. Choose the pronunciation
in the actual context, including ambiguous characters and inflected forms.
A card's reading belongs to its precise displayed original form; do not derive
it by slicing sentence pinyin or reading a different dictionary form. Keep
readings in their dedicated fields. Speech and alignment use only the original
target-language text, never readings or constituent explanations.

Use the CLI's stable token indices to author meaningful whole-word groups.
Mandarin character tokens are indexing units, not automatically words: group
`火车` as `huǒchē` / `train`, then explain its constituents `火` / `huǒ` / `fire`
and `车` / `chē` / `vehicle`. The whole-word meaning is primary; constituent
meanings explain its parts and need not add up to its contextual meaning.
Supply useful parts for other languages where appropriate; do not invent a
decomposition for every word or guess word boundaries from sentence readings.

Generate these fields during the existing draft and vocabulary stages, including
requested revisions. Regenerating original text also regenerates its associated
readings and breakdowns, bound to that exact text. A translation-only change
does not invalidate a sentence reading; it can still require updated vocabulary.
Use the existing preparation/validation flow for incomplete or stale output.
Do not add a separate pronunciation approval, manual review or direct editing
workflow. Validation checks source identity and completeness; it cannot prove
pronunciation accuracy. Readings alone do not establish speech-provider or
MMS-FA support for a language or variety.

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

### Stable vocabulary identities

Author `countingForm` for every token and whole-word group alongside its
contextual analysis. It is the plain dictionary headword in the original
writing system, not the displayed inflection, translation or transliteration.
Use the same headword across lessons and inflections: Croatian `pijem` and
`piješ` both use `piti`; Mandarin `喜欢` uses `喜欢`, not `xǐhuan`.
Retain an intrinsic reflexive particle when it is part of the lexical headword.
Keep gender, noun class, part-of-speech labels, aspect partners, pronunciation,
parentheses and explanatory notes in `lemma`, `pos`, `form` or `note`, not in
`countingForm`. Do not invent a compound headword to combine unrelated words.

Author all multi-character word groups correctly. `喜欢` is one whole word;
its constituent `喜` and `欢` still have their own readings, explanations and
counting forms for the lookup display, but are not additional word encounters
when the group is present. Separate single-character words remain separate.
Expressions also need `countingForm`: their plain original-script citation form
or construction pattern, separate from any argument or grammar annotations in
`canonical`. Use one `…` for a variable slot: `是…吗`, `和…一起`, or `i … i …`.
Keep fixed words and intrinsic particles; omit labels such as `(+ dat.)` or
`(impf.)`. Their existing readings remain bound to the exact displayed
`canonical`, not to the counting form. Expressions are counted separately from
words; shared indices must not inflate word totals.
Punctuation and standalone numeric tokens do not establish vocabulary items.

These identities support counts of distinct words heard and repeated word
exposures, not claims that a learner has mastered the vocabulary. A metadata-only
repair must preserve the original text, translations, sentence IDs, readings,
audio, alignment and existing practice history. Do not strip annotations with
a heuristic and call the result reviewed: resolve the actual headword from
the sentence and its contextual analysis. Flag genuine ambiguity for review.
Never reimport a lesson as a duplicate merely to update its vocabulary metadata.

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
