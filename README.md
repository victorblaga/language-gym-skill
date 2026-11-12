# Language Gym generation skill

Create language lessons with your own local agent and import complete private
lessons into a Language Gym app. This repository contains the reusable skill,
using the open `SKILL.md` format for Codex, Claude Code, Antigravity and other
compatible harnesses with local shell access.

The agent interviews you or adapts source material and writes the lesson and
contextual vocabulary. The standalone Language Gym CLI handles speech,
alignment, validation, ZIP packaging and optional app access. Text uses your
agent's own model access; speech uses your local credentials or an existing
recording. The hosted app does not pay for generation.

## Install

Copy or symlink the entire `skills/generate-lesson` folder, including references:

| Harness | Location | Invocation |
| --- | --- | --- |
| Codex | `~/.agents/skills/generate-lesson/` | `$generate-lesson` |
| Claude Code | `~/.claude/skills/generate-lesson/` | `/generate-lesson` |
| Antigravity | Project `.agents/skills/generate-lesson/` or global `~/.gemini/config/skills/generate-lesson/` | `/generate-lesson` |
| Other compatible agents | Their documented skills directory | Their skill selector |

For example, install for Codex without replacing an existing skill:

```sh
git clone https://github.com/victorblaga/language-gym-skill.git
mkdir -p ~/.agents/skills
ln -s "$PWD/language-gym-skill/skills/generate-lesson" ~/.agents/skills/generate-lesson
```

Optional `agents/openai.yaml` supplies Codex UI metadata; the workflow has no
Codex-only runtime. Current discovery documentation:
[Codex](https://learn.chatgpt.com/docs/build-skills),
[Claude Code](https://code.claude.com/docs/en/skills),
[Antigravity](https://antigravity.google/docs/skills).

## CLI and alignment

Get the compatible standalone CLI from your Language Gym app's Settings, or:

```text
https://YOUR_LANGUAGE_GYM_APP/downloads/language-gym-lesson.zip
```

Extract it and run `node /path/to/language-gym-lesson.mjs help`, then `doctor`.
It requires Node 24+ and ffprobe. The download includes the local MMS-FA aligner
source and exact Docker setup in `aligner/README.md`. A copied workspace can
adopt existing audio, align, validate and pack without an app account or paid
model call once the local tools/model are installed.

This public repository contains instructions and format examples. The app
distributes the compatible CLI implementation; it does not need to expose its
private server implementation or learner data.

## Optional app connection

Run `auth login --app https://YOUR_LANGUAGE_GYM_APP`. Sign into the app in your
browser, check the account and matching connection code, then approve. On a
remote machine use `--no-browser` and open the printed link yourself. Choose:

- `context:read` for selected-language level, recorded facts, islands and lesson
  summaries.
- `lessons:import` for adding complete private lessons.

The CLI saves the token in protected config outside lesson workspaces, bound to
the exact app origin. Use `auth status` to check it and `auth logout` to revoke
it. Manually created Settings tokens can still be supplied through CLI stdin,
never a command argument. Tokens
expire and can be revoked; they grant no Studio, public publication, account
management or practice-progress permissions.

The agent discovers your saved connection with `auth status`, reads your level
and learning context, prepares the lesson locally, then imports it privately by
default. It returns the lesson title and app link while keeping the ZIP as a
backup. Ask for a ZIP only or no upload to keep the work local. Multiple saved
apps require an explicit choice; a read-only connection cannot upload.
ZIPs exclude credentials, source interviews and private provider-operation files.

## Try it

Ask your agent: “Use generate-lesson to interview me about describing my job in
Croatian at B2. Let me develop connected answers, use natural everyday speech,
and show me the draft before audio. Use my chosen speech provider with an
explicit budget, then add the finished lesson to my connected Language Gym account.”

Existing text and recordings are supported too. Honor explicit requests to
preserve an original rather than adapting it into a new dialogue.

The CLI supports local Gemini, OpenAI Realtime and official Azure Speech, plus
audio from other available tools. An explicit `--env-file PATH` loads your own
API keys locally; app login and speech credentials are separate. Two-speaker
Gemini lessons automatically use your profile's male/female voice pairing,
defaulting to Orus and Kore when no profile defaults exist. Choose voices using actual language support and listening
preferences; do not assume a subscription grants API credentials or credits.
See [speech choices](skills/generate-lesson/references/speech.md) and
[workspace examples](skills/generate-lesson/references/workspace.md).
