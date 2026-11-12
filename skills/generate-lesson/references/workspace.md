# Workspace files

Run `init` first. Keep the generated `workspace.json` version, bundle ID and
creation time. Its language, level and method must agree with the draft. Use
full language names for a readable import preview (`--language-name Croatian
--base-name English` at initialization). Do not add credentials or app account
IDs to this file.

`draft.json` is an untimed lesson. Here is a small conversation showing the
required structure; a normal lesson contains more material:

```json
{
  "title": "Kava nakon posla",
  "description": "Arranging a coffee with a friend.",
  "kind": "dialogue",
  "method": "conversation",
  "speakers": {
    "partner": "Ana", "learner": "Victor",
    "partnerGender": "female", "learnerGender": "male"
  },
  "voices": { "primary": "Orus", "secondary": "Kore" },
  "sentences": [
    { "id": "s1", "paragraph": "p1", "speaker": "Ana", "turnRole": "prompt",
      "text": "Idemo na kavu?", "translation": "Shall we get coffee?" },
    { "id": "s2", "paragraph": "p1", "speaker": "Victor", "turnRole": "response",
      "text": "Može, imam vremena.", "translation": "Sure, I have time." },
    { "id": "s3", "paragraph": "p1", "speaker": "Victor", "turnRole": "response",
      "text": "Danas sam slobodan. Idemo odmah.",
      "translation": "I am free today. Let’s go now." }
  ]
}
```

Use unique stable sentence IDs. Paragraph IDs must be contiguous: once a new
paragraph starts, do not return to an earlier one. Every dialogue row names one
of the mapped speakers and uses that person's role, including their questions.
For a monologue use `kind`/`method` equal to `monologue`, only a primary voice,
and omit speaker/role fields. Cups uses `method: "cups"` and its optional
`round`, `cup`, `patternId` teaching metadata. The CLI does not write content or
invent timings. Never put `start`, `end` or `matchConfidence` into the draft.

Run `tokens DIR --pending --limit 5` after writing the draft. It emits token indices and the exact
24-character sentence hash for the current text plus translation. `lexicon.json`
maps every sentence ID to `{ "sentenceHash": "...", "lexicon": {...} }`.
For the first example above, a complete entry has this shape:

```json
{
  "s1": {
    "sentenceHash": "2aa959302f22062ff5402ae0",
    "lexicon": {
      "sentenceId": "s1",
      "words": [
        { "i": 0, "gloss": "shall we go", "lemma": "ići", "pos": "verb",
          "form": "1st person plural present" },
        { "i": 1, "gloss": "for", "lemma": "na", "pos": "preposition",
          "form": "with accusative", "exprIds": ["coffee"] },
        { "i": 2, "gloss": "coffee", "lemma": "kava", "pos": "noun",
          "form": "accusative singular", "exprIds": ["coffee"] }
      ],
      "expressions": [
        { "id": "coffee", "indices": [1, 2], "canonical": "na kavu",
          "meaning": "for a coffee" }
      ]
    }
  }
}
```

This hash matches the exact first example; use `tokens` for your own draft and
add complete entries for every other sentence. Cover every token in order; do not omit function words. Each
word requires a contextual gloss, lemma, part of speech and grammatical form.
Expressions require unique local IDs, sorted token indices, a canonical form
and contextual meaning. Shared and split expressions may overlap. A word's
optional `exprIds` must name expressions containing that word's index; indices
alone also link an expression. Do not attach stale lexicons to changed content
by copying a new hash without reviewing the words and expressions.

Write a batch file in that same sentence-ID-to-entry map shape, then run
`lexicon merge DIR --file BATCH.json`. The CLI checks every entry against the
current draft before saving any of them and retains existing completed rows.
`tokens DIR --pending --limit 5` then returns the next unfinished batch; changed
text/translations and invalid rows are pending too. `sentenceCount`,
`completeCount` and `remainingCount` describe the whole draft, even when only
five sentences are returned. Keep batching until none remain, then validate.

Run `validate DIR` before audio work. After `audio --file` or `render`, use
`align` to generate `alignment-input.json`, `alignment.json` and timed
`lesson.json`. Then `validate DIR --ready` and `pack DIR --output FILE.zip`.
All authoritative export files are selected by the CLI; never add `.local`,
source interviews, credentials or provider response files to the ZIP yourself.
