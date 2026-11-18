# Language Gym Skill Setup

Follow these steps in order when asked to install the Language Gym skill, set up
the `language-gym-lesson` CLI, or connect a user's Language Gym account. Live
agent discovery entrypoints on Language Gym: `https://languagegym.app/llms.txt`
and `https://languagegym.app/.well-known/agent-skills/index.json`.

**Idempotency first:** Inspect existing skill symlinks, `language-gym-lesson` on
`PATH`, local aligner health, and `auth status` before overwriting anything. Skip
steps that are already working unless the user asks to reinstall or update.

## 1. Install the `generate-lesson` skill

Check whether a working `generate-lesson` skill is already installed for the
active harness. Otherwise install via `npx` if available, or fall back to the
zero-dependency `git clone` and symlink steps below.

**With `npx`:**

```sh
npx skills add victorblaga/language-gym-skill@generate-lesson
```

**Without `npx` (or from an existing checkout):**

Check whether the current directory is already a checkout of `language-gym-skill`
(`skills/generate-lesson/SKILL.md` exists). Otherwise, clone into
`~/.local/share/language-gym-skill` (or update an existing clone):

```sh
if [ -d "$HOME/.local/share/language-gym-skill/.git" ]; then
  git -C "$HOME/.local/share/language-gym-skill" pull --ff-only
else
  mkdir -p "$HOME/.local/share"
  git clone https://github.com/victorblaga/language-gym-skill.git "$HOME/.local/share/language-gym-skill"
fi
```

Symlink `<repo>/skills/generate-lesson` (which includes `references/` and
`agents/`) into the active agent's user skills directory. Do not replace an
unrelated custom directory that is not already a `generate-lesson` symlink
without asking first. If multiple harness directories already exist on the
machine, symlink into each present one so switching agents works seamlessly:

| Harness | Config check | Skill symlink target |
| --- | --- | --- |
| Claude Code | `~/.claude` | `~/.claude/skills/generate-lesson` |
| Codex / `.agents` | `~/.agents` | `~/.agents/skills/generate-lesson` |
| Cursor | `~/.cursor` | `~/.cursor/skills/generate-lesson` (or workspace `.cursor/rules/` if requested per-project) |
| Antigravity | `~/.gemini/config` | `~/.gemini/config/skills/generate-lesson` |

Example:

```sh
SKILL_SRC="$HOME/.local/share/language-gym-skill/skills/generate-lesson"
mkdir -p "$HOME/.claude/skills"
ln -sfn "$SKILL_SRC" "$HOME/.claude/skills/generate-lesson"
```

## 2. Install the standalone `language-gym-lesson` CLI

1. Verify Node **24 or newer** (`node -v`). If `node` is missing or older than
   v24, stop and give the user the exact Node 24+ install command for their OS.
2. Check whether `language-gym-lesson help` already works on `PATH`. If missing
   or updating, download and unpack the standalone CLI from
   `https://languagegym.app/downloads/language-gym-lesson.zip` (or
   `<APP_URL>/downloads/language-gym-lesson.zip` if the user specifies a custom
   app URL):

```sh
mkdir -p "$HOME/.local/share/language-gym-lesson" "$HOME/.local/bin"
TMP_ZIP="$(mktemp "${TMPDIR:-/tmp}/language-gym-lesson.XXXXXX")"
curl -fsSL https://languagegym.app/downloads/language-gym-lesson.zip -o "$TMP_ZIP"
unzip -oq "$TMP_ZIP" -d "$HOME/.local/share/language-gym-lesson"
chmod +x "$HOME/.local/share/language-gym-lesson/language-gym-lesson.mjs"
ln -sfn "$HOME/.local/share/language-gym-lesson/language-gym-lesson.mjs" "$HOME/.local/bin/language-gym-lesson"
rm -f "$TMP_ZIP"
```

Verify with `language-gym-lesson help`. If `~/.local/bin` is not on `PATH`, run
`"$HOME/.local/bin/language-gym-lesson"` for the remaining steps and tell the user
how to add `~/.local/bin` to their shell profile.

## 3. Verify local tools (`doctor`, `ffmpeg`, Docker, MMS-FA aligner)

Run:

```sh
language-gym-lesson doctor
```

- **`ffmpeg` and `ffprobe`:** If either reports `available: false`, stop or tell
  the user the exact package install command for their OS (`brew install ffmpeg`,
  `sudo apt install ffmpeg`, etc.).
- **Local MMS-FA aligner:** Check `localAligner` in the `doctor` output.
  - If `localAligner.available` is `true` (`status: "ready"`), reuse it.
  - Otherwise, if Docker (`docker.composeAvailable` and `docker.daemonAvailable`)
    is ready, locate the `aligner/` directory beside the installed
    `language-gym-lesson.mjs` binary and follow `aligner/README.md` to start the
    local loopback aligner:

    ```sh
    CLI_BIN="$(command -v language-gym-lesson || printf '%s' "$HOME/.local/bin/language-gym-lesson")"
    CLI_REAL="$(node -e 'console.log(require("fs").realpathSync(process.argv[1]))' "$CLI_BIN")"
    cd "$(dirname "$CLI_REAL")/aligner"
    docker compose up -d --build --wait --wait-timeout 1800
    ```

    Warn the user that the first build/startup downloads the ~1.26 GB Meta MMS-FA
    model and may take several minutes, then re-run `language-gym-lesson doctor`
    to confirm `localAligner.available: true` (`status: "ready"`).
  - If Docker is not installed or the daemon is not running yet, inform the user
    that Docker is required before aligning audio (not needed for account
    login) and continue to Step 4.

## 4. Connect the user's Language Gym account

Default app URL is `https://languagegym.app` unless the user specifies another
`--app URL`.

1. Check existing credentials first:

   ```sh
   language-gym-lesson auth status --app https://languagegym.app
   ```

   - If `connected: true` and both `context:read` and `lessons:import` scopes
     are present, keep the active connection and report it.
   - If a stale, `expired-or-revoked`, or partial-scope connection is stored,
     clear it first (`language-gym-lesson auth logout --app https://languagegym.app`,
     or `auth clear --app https://languagegym.app` if already revoked on the
     server) because `auth login` refuses to overwrite an existing saved entry.
2. Otherwise run `auth login` **in the background** — do not wait synchronously
   for it to exit first, because `auth login` keeps an in-memory PKCE
   `codeVerifier` and blocks polling for up to 10 minutes while waiting for
   browser approval (killing or timing out the command discards the pending
   login):

   - Run it with your harness's background process runner or redirect output to
     a temporary log (`--no-browser` on remote, container, or headless hosts):

     ```sh
     LOGIN_LOG="$(mktemp "${TMPDIR:-/tmp}/lg-login.XXXXXX")"
     language-gym-lesson auth login --app https://languagegym.app >"$LOGIN_LOG" 2>&1 &
     LOGIN_PID=$!
     ```

   - Wait 1–2 seconds, read `$LOGIN_LOG`, and present the printed
     `/connect/cli?...` approval URL and verification code to the user right
     away while keeping `$LOGIN_PID` alive so they can sign in and click
     **Approve** in their browser.
   - Once approved and `$LOGIN_PID` exits `0`, remove `$LOGIN_LOG`.
3. Verify the saved connection:

   ```sh
   language-gym-lesson auth status --app https://languagegym.app
   ```

## 5. Hand off to lesson authoring

Once installed and connected, follow
[`skills/generate-lesson/SKILL.md`](skills/generate-lesson/SKILL.md) to
interview the learner or adapt source material, generate vocabulary/audio, align
timing, pack the lesson ZIP, and import it privately into Language Gym. Never
store API keys or connection tokens inside lesson workspaces or shell arguments.
