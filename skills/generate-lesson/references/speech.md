# Choose speech for this language

Verified 12 November 2026. Provider access, credits and models can change; check
current primary documentation when the available configuration differs. Keep
credentials on the learner's machine and never infer a free balance from a
subscription name. CLI `voices` describes implemented support; `doctor` checks
local prerequisites without rendering a lesson.

## Choose by evidence

First use a previously approved language/voice preference or supplied recording.
Otherwise compare actual language coverage, credential availability, intended
speaker count, budget and downloadable output. For quality, judge pronunciation
and stress, colloquial phrasing, conversational prosody, pace and consistent
speakers. Provider branding or a model's size is not a listening comparison.

For an audition use the same short target-language passage for each candidate:
include a name, a question, a colloquial expression and a connected answer. Get
approval for the sample budget and reuse the resulting files. Have the learner
choose when quality is subjective. Do not silently change providers or switch
to a paid model when another tool fails.

For first-use setup, check `doctor` and ask one provider decision if the user
has not already made it. Offer suitable choices rather than requiring Google:
Gemini for native two-speaker recording with the user's own API/Cloud access,
another configured paid provider, or no-key online Edge speech / existing audio.
Then help set up only the chosen route. A provider choice remains valid on
resume; a setup check is not another audition or spending approval.

## Implemented local providers

**Gemini:** `gemini-3.8-flash-tts` is the quality-focused default; Flash-Lite is a
lower-cost option. Croatian, Chinese and English are explicitly supported.
Gemini supports a continuous recording with up to two prebuilt speakers. Use
structured speaker/style metadata per part, never spoken `Speaker1:` prefixes.
For the existing Croatian pairing use Orus for the male voice and Kore for the
female voice, unless the learner chooses another supported voice.

Google has separate access routes. Choose explicitly:

- **Vertex AI with an existing Google Cloud login:** install Google Cloud CLI
  and run `gcloud auth login` if not already signed in. Use `render DIR
  --provider gemini --gemini-auth gcloud --vertex-project YOUR_PROJECT
  --budget-usd 0.90`. This obtains a short-lived token from that local login;
  it needs no AI Studio key and uses the selected project's Cloud billing.
- **Vertex AI with Application Default Credentials:** use `--gemini-auth adc
  --vertex-project YOUR_PROJECT` when the user already has ADC configured.
  A normal `gcloud auth login` and ADC are different credential setups.
- **Gemini Developer API / AI Studio:** create a key for the intended project,
  save it outside the workspace as `GEMINI_API_KEY`, then use `--gemini-auth
  api-key --env-file /path/to/keys.env`. Paid API access may require positive
  AI Studio prepaid funds even when Cloud billing and eligible promotional
  credits are present. HTTP 402 calls for checking that billing setup.

Keep the Vertex project explicit; never inherit the hosted app's project or
credentials. The user's chat quota is not API authentication.
Free-tier availability and remaining quota are project-specific. Google AI plans
can include claimable developer Cloud credits; verify that the user has linked
the relevant billing account. [Speech and languages](https://aistudio.google.com/docs/speech-generation),
[pricing](https://aistudio.google.com/docs/pricing),
[subscription developer benefits](https://developers.google.com/profile/help/benefits).
[AI Studio billing](https://ai.google.dev/gemini-api/docs/billing#prepay),
[Google Cloud login](https://docs.cloud.google.com/sdk/docs/authorizing).

**Azure Speech:** use the learner's Speech resource key and region. Native
Croatian choices include `hr-HR-GabrijelaNeural` and `hr-HR-SreckoNeural`; inspect
the official voice list for other locales. Use only styles that the selected
voice actually supports. Azure's F0 allowance is currently 500,000 Neural
characters/month, but a key does not prove that the resource uses F0 or has
remaining allowance. If a paid price is unknown, state it and require the CLI's
explicit estimate/budget input rather than labelling the recording free.
[Voice catalogue](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/language-support?tabs=tts),
[pricing](https://azure.microsoft.com/en-us/pricing/details/speech/),
[official REST API](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/rest-text-to-speech).

**OpenAI:** `--provider openai` uses `gpt-realtime-2.1-mini` through the Realtime
API with a local `OPENAI_API_KEY`. Inspect `voices --provider openai --language
hr`, then choose explicit learner and partner voices. Each contiguous speaking
turn uses a fresh, bounded response; the CLI checks its returned transcript
against the supplied text before accepting the audio. Saved responses can be
recovered without buying them again. A mismatch or unknown paid outcome stops
rendering; do not discard the ledger to retry.

For example, use `render DIR --provider openai --learner-voice cedar
--partner-voice marin --budget-usd 0.90 --env-file /path/to/keys.env`. Keys stay on
the learner's machine. Claimed subscription credits still require the matching
API organization's credentials and available model access; a model listing
alone does not verify a credit balance. The older `tts-1`, `tts-1-hd` and
`gpt-4o-mini-tts` Speech API models retire on 6 January 2027; this adapter uses
the documented Realtime workflow.
[Speech guide](https://developers.openai.com/api/docs/guides/text-to-speech),
[Realtime conversations](https://developers.openai.com/api/docs/guides/realtime-conversations),
[pricing](https://developers.openai.com/api/docs/pricing).

## Other available tools

If an actual host tool can return a downloadable recording with the required
voices/language, it can supply the audio. Preserve the tool/model provenance
and adopt the result with the CLI. Plain text output or consumer voice playback
is not evidence that an exportable audio file exists.

**Edge speech:** offer `edge-tts` as a no-key, no-paid-account online option.
It uses Microsoft's consumer Read Aloud endpoint through an unofficial
integration. Availability can change; it is not an offline model or a supported
Azure Speech resource. After the user chooses it, use an installed `edge-tts`
or `uvx --from edge-tts edge-tts` (requires uv/Python) and inspect `--list-voices`
for the target language. Do not switch a user who already chose paid Google.

Edge accepts one voice per request. For a dialogue, group adjacent chunks from
the same speaker, write their exact text without speaker labels to numbered
turn files, and generate each with its selected male/female voice. Preserve
completed recordings on resume. For example:

```sh
edge-tts --list-voices
edge-tts --voice SELECTED_VOICE --file turn-001.txt --write-media turn-001.mp3
```

Write `turns.txt` with ordered lines such as `file 'turn-001.mp3'`, then assemble
the final recording once and adopt it:

```sh
ffmpeg -f concat -safe 0 -i turns.txt -ar 24000 -ac 1 -c:a pcm_s16le recording.wav
language-gym-lesson audio DIR --file recording.wav
language-gym-lesson align DIR --aligner http://127.0.0.1:8091
```

Check the joins and voice assignments. Align the assembled recording; Edge's
SRT output does not replace real MMS-FA alignment. The CLI's native `--provider`
list stays Gemini/Azure/OpenAI; Edge enters through `audio --file`.
[Upstream tool](https://github.com/rany2/edge-tts),
[Microsoft Read Aloud](https://support.microsoft.com/en-us/edge/use-immersive-reader-in-microsoft-edge).

## Honest accounting

The agent's interview, draft and lexicon use its existing tools/subscription.
Speech may incur a separate API charge even when credits offset it. Every paid
render or audition needs a declared ceiling; report the recorded amount as
known, estimated or unknown. A timeout after dispatch is an unknown outcome,
not permission to buy the same recording again. Existing audio and local
MMS-FA require no speech-provider call.
