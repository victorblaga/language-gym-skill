# Spoken lesson authoring

Use these rules for new drafts, requested adaptations and contextual vocabulary
going forward; leave recorded existing content (such as existing Croatian
lessons) untouched unless a revision is requested. An approved draft stays
approved; continue its unfinished preparation and delivery without silently
rewriting it. Preserve supplied wording when faithful preservation was
requested.

## Everyday register

Every sentence must pass the “Say It Tomorrow” test: a native speaker would say
it in the practical situation, and the learner could reuse it tomorrow, perhaps
by swapping one word. Use everyday vocabulary, idiomatic chunks, conversational
particles, natural word order and the target language's reflexive/phrasal
constructions. Higher levels add fluent reasons, conditions, clarification and
reactions; they do not call for rarer or more literary vocabulary.

Keep practice chunks breathable within the target level's per-chunk cap (`<= 6`
words / `<= 10` CJK characters at `A0`, scaling up to `<= 15` words / `<= 26`
CJK characters at `C1–C2`). Short natural questions and reactions can be
shorter. Split long explanations at natural sentence boundaries. Exclude poetic
metaphors, scenery clichés, bureaucratic phrasing, purist dictionary calques and
multi-clause essay sentences. In Croatian, use everyday words such as `laptop`, `mobitel`, `auto` and `dućan`, natural chunks
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

## 1-2-3 Cups (`A0–A2`)

For `A0` (`absolute_beginner`), `A1`, and `A2` dialogues (and `unknown` level),
`method: "cups"` is mandatory unless an explicit format or faithful-preservation
request applies. Size each lesson to the target level:

- `A0`: `16–20` chunks (default `18`, `3` rounds × `6`; Lesson 1 of a brand-new
  language may use `12` chunks / `2` rounds), `<= 6` words / `<= 10` CJK chars
  per chunk, `4–6` new words.
- `A1`: `18–22` chunks (default `20`, `3` rounds), `<= 8` words / `<= 13` CJK
  chars per chunk, `6–8` new words.
- `A2`: `20–24` chunks (default `24`, `4` rounds), `<= 10` words / `<= 16` CJK
  chars per chunk, `8–10` new words.

Build each round as a connected 6-Cup chain (`round` is an integer `>= 1`: `3`
rounds at `A0–A1`, `2` rounds for a 12-chunk `A0` Lesson 1, `4` rounds at `A2`),
tagging each sentence's `cup` (and optional `patternId`):

1. `statement` (Cup 1 · Statement): partner models a concrete everyday line or
   core pattern.
2. `variation` (Cup 1 · Variation): learner transforms the frame (person,
   location, time, object, or polarity swap).
3. `yes_no_question` (Cup 2 · Yes/No Q): partner asks a natural yes/no question
   testing the frame.
4. `response` (Cup 2 · Response): learner answers directly and carries the
   frame forward.
5. `open_question` (Cup 3 · Open Q): partner asks an open question
   (`who`/`what`/`where`/`when`/`which`/`how`).
6. `bridge` (Cup 3 · Bridge): learner answers and naturally bridges into the next
   detail or round.

Use optional `negative_contrast` (negation/contrast of the active frame) or
`extension` (a short natural follow-up) when a round includes an extra beat
(such as a `20`-chunk `A1` lesson).

Follow these non-negotiable Cups rules:

- **Strict `1:1` speaker alternation**: adjacent sentences must alternate
  speakers (`s[i].speaker !== s[i - 1].speaker`) and roles, with `1` sentence
  per turn.
- **Named local peer partner**: cast a realistic named peer suited to the
  setting (friend, neighbor, barista, coworker) — never `"Teacher"` or `"Tutor"`.
- **`4–6×` structural frame transformations**: recycle every new core word and
  pattern `4–6×` across genuine transformations (person swap, yes/no question,
  negation/contrast, open question, bridge) while staying idiomatic. Never write
  back-to-back verbatim monologue loops (`s[i].text !== s[i - 1].text`).
- **Strict per-sentence caps**: every sentence must obey the level's word and
  CJK character limit.
- **Spiral review every 5th lesson**: Lessons 5, 10, 15, 20, ... are spiral
  review 1-2-3 Cups lessons (`0` new words, or at most `1–2` glue words) that
  recombine earlier lessons naturally.

## One cohesive conversation (`B1–C2`)

For a new `B1–C2` dialogue, write one continuous, realistic interaction in the
recorded setting between exactly two speakers, sized to the level:

- `B1`: `20–25` chunks (default `22`), `<= 11` words / `<= 18` CJK chars per
  chunk, `8–12` new words/collocations, `1–3` consecutive sentences per turn.
- `B2`: `22–26` chunks (default `25`), `<= 12` words / `<= 22` CJK chars per
  chunk, `10–14` new words/collocations, `1–4` consecutive sentences per turn.
- `C1`: `28–34` chunks (default `30`), `<= 15` words / `<= 26` CJK chars per
  chunk, `12–16` new words/collocations, `1–4` consecutive sentences per turn.
- `C2`: `28–34` chunks (default `32`), `<= 15` words / `<= 26` CJK chars per
  chunk, `12–16` new words/collocations, `1–4` consecutive sentences per turn.

Sentence count is separate from turn count. Keep each chunk to `1` main clause
plus at most `1` short subordinate clause, and vary answer length within the
level's turn cap (`1–3` consecutive sentences at `B1`, `1–4` at `B2–C2`) when an
answer develops a reason, concrete example, or distinction; stop when the idea
is complete. Echo new words and collocations `2–3×` naturally across partner
prompt → learner answer → follow-up. The learner should carry roughly two thirds
to three quarters of the words when the situation fits. The partner reacts and
follows up naturally; do not insert another interview question after every
sentence. Learner questions and brief clarifications are welcome.

Use five contiguous beats, normally `p1` through `p5`, distributed naturally
across the level's chunk target without padding or forcing a speaker change at a
boundary:

1. Open the interaction naturally in its real-life setting.
2. Exchange concrete updates, context or what is needed now.
3. Clarify, explain how something works or compare practical options.
4. Express a preference or handle a small condition, obstacle or alternative.
5. Make a concrete decision or plan, or close naturally.

For new conversation drafts, use one sentence per `sentences` entry for practice
and alignment. A developed answer spans consecutive entries by the same speaker.
Retain an approved draft's existing chunks, including short combined sentences.
An explicitly chosen monologue is casual first-person speech across a table,
with a similar practical progression. Cups uses its connected `1:1` pattern
rounds; do not impose conversation turn lengths on `method: "cups"`.

## Casting and facts

Cast the learner and one local peer interlocutor suited to the situation (never
`"Teacher"` or `"Tutor"`), using known names accurately. Use only recorded or
supplied biography; for generic lessons use relatable details without a real
learner's private facts. Keep grammatical gender consistent both for a speaker's
own speech and for the person addressed.

Every dialogue entry repeats its explicit speaker name and fixed role:
`speakers.learner` always has `turnRole: "response"`, including their questions;
`speakers.partner` always has `turnRole: "prompt"`, including their reactions.
Roles follow names, never row parity. In `conversation`, keep the same name and
role across a multi-sentence answer; in `cups`, enforce strict `1:1` speaker
alternation with `1` sentence per turn. Voice bindings remain consistent with
those speakers; the CLI handles provider request construction.

## Complete contextual vocabulary

Use the CLI's exact token indices and sentence hashes. For every token,
including function words, supply a concise in-context gloss or grammatical role
in the base language, `lemma`, `pos` and `form`. Glosses normally use 1–5 words;
give the meaning in this sentence instead of a dictionary list. Adapt analysis
to the target language: citation forms, intrinsic reflexivity/separability,
verb aspect where relevant, noun gender/class, and helpful script readings.
Explain the actual inflected or syntactic form and what governs it, such as
case after a preposition, person/tense, agreement or a clitic's role. For
purely grammatical tense/mood helpers (for instance Croatian `sam/si/je/smo/ste/su`,
`ću/ćeš`, `bih/bi`), write a helper gloss such as `"past helper (1st sg.)"` or
`"future helper (2nd sg.)"`, never a bare subject pronoun like `"I"` or `"they"`.

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

Capture only true multi-word dictionary units and split periphrastic verb pairings in
`expressions`, never whole clauses or free modifiers:

- `"kind": "lexical"` (omitted defaults to `"lexical"`; counted in expression vocabulary):
  intrinsic reflexive/pronominal verbs (`dogoditi se`), verbs/adjectives with a
  fixed governed preposition **stopping at the preposition** (for `"A što se
  dogodilo sa starim tvornicama?"`, annotate `[2, 3, 4]` as `"dogoditi se s"`,
  never `"što se dogodilo s tvornicama"`), separable/phrasal/light-verb units,
  fixed connectors/correlatives (`na miru`, `和…一起`, `是…吗`), lexicalized
  compounds (`你好`), and true non-literal idioms/sayings. When a `"lexical"`
  unit appears in a compound tense (`Proizvodili su se`), include the auxiliary
  clitic (`su`) inside that single `"lexical"` entry instead of adding a
  duplicate `"grammar"` entry.
- `"kind": "grammar"` (highlighted for learners as a single unit with a
  `grammar` badge, but excluded from expression vocabulary so its helper and verb
  still count as separate words): split periphrastic verb inflections where an
  auxiliary/helper clitic carries tense or mood (`prešao sam` → `canonical: "prijeći"`,
  `countingForm: "prijeći"`; `ćeš popiti` → `popiti`) or discontinuous particles
  like `慢一点` (`countingForm: "…一点"`).
- **Explicit exclusions**: never create an expression for free subjects,
  objects, or modifiers; modal/aspect verb + infinitive (`može prepoznati`,
  `počele propadati`); copula `biti` + free adjective/noun (`biti živ`,
  `bila stroga`); ordinary preposition + noun (`sa starim tvornicama`,
  `u svakodnevnom životu`); literal verb + object (`喝茶`, `踢足球`); literal
  negation + verb (`不喜欢`); or multi-character CJK single words (`喜欢`, `足球`
  belong in `groups`, not `expressions`).

Include split or discontinuous parts using sorted, possibly nonadjacent indices.
Shared clitics, auxiliaries or prepositions and nested constructions can belong
to several entries; include the shared index in every entry it actually
participates in. Link each participating word through `exprIds`, retaining its
own token gloss and morphology. Add a literal explanation only for a
nontransparent expression, and a note only when it explains useful grammar,
register or scope.

Use [workspace files](workspace.md) for the saved schema and resumable merge
commands. Complete vocabulary is required even for an existing recording or a
faithfully preserved source.
