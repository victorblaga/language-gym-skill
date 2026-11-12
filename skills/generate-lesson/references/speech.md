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

## Implemented local providers

**Gemini:** `gemini-3.8-flash-tts` is the quality-focused default; Flash-Lite is a
lower-cost option. Croatian, Chinese and English are explicitly supported.
Gemini supports a continuous recording with up to two prebuilt speakers. Use
structured speaker/style metadata per part, never spoken `Speaker1:` prefixes.
For the existing Croatian pairing use Orus for the male voice and Kore for the
female voice, unless the learner chooses another supported voice.

Use an explicitly configured Gemini API key or explicit Vertex project with
Application Default Credentials. The user's chat quota is not API authentication.
Free-tier availability and remaining quota are project-specific. Google AI plans
can include claimable developer Cloud credits; verify that the user has linked
the relevant billing account. [Speech and languages](https://aistudio.google.com/docs/speech-generation),
[pricing](https://aistudio.google.com/docs/pricing),
[subscription developer benefits](https://developers.google.com/profile/help/benefits).

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

An already installed `edge-tts` may be offered explicitly as a best-effort
external-audio option. Inspect its help and available voices. It uses Microsoft's
consumer Read Aloud endpoint through an unofficial integration, rather than
Azure's supported Speech API; availability and export permissions must not be
assumed from the Edge browser feature. Do not install or choose it silently.
Its SRT output still needs real MMS-FA alignment for this app.
[Upstream tool](https://github.com/rany2/edge-tts),
[Microsoft Read Aloud](https://support.microsoft.com/en-us/edge/use-immersive-reader-in-microsoft-edge).

## Honest accounting

The agent's interview, draft and lexicon use its existing tools/subscription.
Speech may incur a separate API charge even when credits offset it. Every paid
render or audition needs a declared ceiling; report the recorded amount as
known, estimated or unknown. A timeout after dispatch is an unknown outcome,
not permission to buy the same recording again. Existing audio and local
MMS-FA require no speech-provider call.
