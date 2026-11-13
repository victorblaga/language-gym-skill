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
        { "i": 0, "gloss": "shall we go", "lemma": "ići", "countingForm": "ići", "pos": "verb",
          "form": "1st person plural present" },
        { "i": 1, "gloss": "for", "lemma": "na", "countingForm": "na", "pos": "preposition",
          "form": "with accusative", "exprIds": ["coffee"] },
        { "i": 2, "gloss": "coffee", "lemma": "kava", "countingForm": "kava", "pos": "noun",
          "form": "accusative singular", "exprIds": ["coffee"] }
      ],
      "expressions": [
        { "id": "coffee", "indices": [1, 2], "canonical": "na kavu", "countingForm": "na kavu",
          "meaning": "for a coffee" }
      ]
    }
  }
}
```

This hash matches the exact first example; use `tokens` for your own draft and
add complete entries for every other sentence. Cover every token in order; do not omit function words. Each
word requires a contextual gloss, lemma, clean original-script `countingForm`,
part of speech and grammatical form. The counting form is the plain dictionary
headword, with no grammar labels, pronunciation or explanatory annotations.
Existing older lessons without this field remain readable; always supply it
when authoring new vocabulary or repairing existing vocabulary metadata.
Expressions require unique local IDs, sorted token indices, a canonical form,
their clean `countingForm` and contextual meaning. Keep one `…` per variable
slot in a construction's counting form, without argument-case or grammar labels.
Shared and split expressions may overlap. A word's
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

## Languages with readings

The selected-language context's `language.romanization` is authoritative:
`{ "scheme": "pinyin", "label": "Pinyin" }`, with optional `variety` for a
chosen spoken variety. `workspace.json` stores the same shape under
`targetLanguage.romanization`. `init --language zh` selects pinyin automatically.
For another agreed convention, use `--romanization-scheme ID
--romanization-label NAME`, and `--variety NAME` where applicable. Arabic
requires both an agreed convention and spoken variety. Reuse the saved setup;
workspace flags do not change the connected learner's language settings, and
an import must agree with those settings.

Add a reading to each draft sentence using the exact original as `sourceText`:

```json
{
  "id": "s1", "paragraph": "p1",
  "text": "我喜欢喝茶。", "translation": "I like drinking tea.",
  "romanization": {
    "scheme": "pinyin", "text": "Wǒ xǐhuan hē chá.",
    "sourceText": "我喜欢喝茶。"
  }
}
```

This example omits dialogue speaker fields only for brevity; retain them in a
dialogue draft. Run `tokens` after authoring the draft. Its `sourceText`, exact
token surfaces, indices and `romanization` configuration describe the material
to annotate; `romanizationRequired` identifies required readings. Carry the
reported `tokenizerVersion` into the lexicon: configured romanized lessons
always use `tokenizerVersion: 2`, including Mandarin. This version preserves
combining marks within tokens. Do not split vocalized Arabic or Mandarin words
independently of the CLI's indices. Existing lessons without a reading
configuration retain their legacy indexing.

For that Mandarin sentence, the batch entry's `lexicon` has this shape. Keep the
`sentenceHash` returned by `tokens` in its surrounding entry as above:

```json
{
  "sentenceId": "s1",
  "tokenizerVersion": 2,
  "words": [
    { "i": 0, "gloss": "I", "lemma": "我", "countingForm": "我", "pos": "pronoun", "form": "subject",
      "romanization": { "scheme": "pinyin", "text": "wǒ", "sourceText": "我" } },
    { "i": 1, "gloss": "to like", "lemma": "喜", "countingForm": "喜", "pos": "bound morpheme", "form": "first character of 喜欢",
      "romanization": { "scheme": "pinyin", "text": "xǐ", "sourceText": "喜" } },
    { "i": 2, "gloss": "joy", "lemma": "欢", "countingForm": "欢", "pos": "bound morpheme", "form": "second character of 喜欢",
      "romanization": { "scheme": "pinyin", "text": "huan", "sourceText": "欢" } },
    { "i": 3, "gloss": "to drink", "lemma": "喝", "countingForm": "喝", "pos": "verb", "form": "uninflected", "exprIds": ["drink-tea"],
      "romanization": { "scheme": "pinyin", "text": "hē", "sourceText": "喝" } },
    { "i": 4, "gloss": "tea", "lemma": "茶", "countingForm": "茶", "pos": "noun", "form": "object", "exprIds": ["drink-tea"],
      "romanization": { "scheme": "pinyin", "text": "chá", "sourceText": "茶" } }
  ],
  "groups": [
    { "indices": [1, 2], "gloss": "to like", "lemma": "喜欢", "countingForm": "喜欢", "pos": "verb", "form": "uninflected",
      "romanization": { "scheme": "pinyin", "text": "xǐhuan", "sourceText": "喜欢" } }
  ],
  "expressions": [
    { "id": "drink-tea", "indices": [3, 4], "canonical": "喝茶", "countingForm": "喝茶", "meaning": "to drink tea",
      "romanization": { "scheme": "pinyin", "text": "hē chá", "sourceText": "喝茶" } }
  ]
}
```

`groups` defines whole words spanning two or more contiguous token indices;
groups must be sorted and cannot overlap. Each group also carries its own clean
`countingForm`, representing the whole word rather than its character count.
Retain their underlying `words` as
the displayed constituent breakdown. Uncovered tokens use their own word
entry, so do not duplicate them as singleton groups. Group `sourceText` is the
exact original surface spanning its indices, including any marks or separators;
word `sourceText` is its exact token surface. Expression `sourceText` equals
its displayed `canonical`, which can differ from the sentence's inflected form.
Every reading's scheme must match the language setup. Author all required
readings before validation; do not copy a new `sourceText` onto an old reading
to bypass staleness checks.

The CLI packages reading-bearing lessons as prepared format v2; older v1-only
readers cannot import them. Keep using the CLI's validation and packing rather
than changing a bundle version or manifest by hand. Existing lessons without
readings retain the v1 contract.

Run `validate DIR` before audio work. After `audio --file` or `render`, use
`align` to generate `alignment-input.json`, `alignment.json` and timed
`lesson.json`. Then `validate DIR --ready` and `pack DIR --output FILE.zip`.
All authoritative export files are selected by the CLI; never add `.local`,
source interviews, credentials or provider response files to the ZIP yourself.
