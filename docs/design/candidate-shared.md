# Candidate A: shared package with runtime adapters

## Problem

VibeWise must run from one repository in Claude Code and Codex without splitting the learning behavior or the three-note state contract. Codex 0.149 adds a native manifest, local marketplace, `PLUGIN_ROOT`, hook trust, four `SessionStart` sources, and Windows `python`. The current Claude package and its reset safety must keep working.

## Usage

Claude users keep installing `vibe-wise` and invoking `/vibe-wise:learn` or `/vibe-wise:reset`.

Codex users add the repository marketplace, enable `vibe-wise@vibe-wise-local`, review its hook with `/hooks`, restart, then invoke `$learn` or `$reset`. On `startup`, `resume`, `clear`, or `compact`, the trusted Codex hook restores the same learning guide and project notes. Reset still previews first, asks in chat for explicit confirmation, and executes only with the returned snapshot token.

```text
codex plugin marketplace add <repository-root>
codex plugin marketplace list
# Enable vibe-wise@vibe-wise-local, trust it in /hooks, then restart Codex.
```

## Shape

```python
Runtime = Literal["claude", "codex"]
StartSource = Literal["startup", "resume", "clear", "compact", "fork"]
NOTE_NAMES = ("profile.md", "progress.md", "project-map.md")

@dataclass(frozen=True)
class StartEvent:
    runtime: Runtime
    cwd: Path
    source: StartSource

@dataclass(frozen=True)
class RestoreDirective:
    additional_context: str

@dataclass(frozen=True)
class ResetPreview:
    project: Path
    state: Path
    files: tuple[str, ...]
    backup_parent: Path
    confirmation: str

def locate_state(cwd: Path) -> Path | None: ...
def plan_restore(event: StartEvent, plugin_root: Path) -> RestoreDirective | None: ...
def parse_claude_start(payload: object) -> StartEvent | None: ...
def parse_codex_start(payload: object) -> StartEvent | None: ...
def render_claude_start(directive: RestoreDirective) -> dict[str, object]: ...
def render_codex_start(directive: RestoreDirective) -> dict[str, object]: ...
def preview_reset(cwd: Path) -> ResetPreview | NoNotes: ...
def confirm_reset(cwd: Path, confirmation: str) -> ResetResult: ...
```

`skills/learn/` owns the single learning policy and note templates. `vibewise/state.py` owns note names, Git-boundary lookup, symlink refusal, activation, and reset fingerprints. `vibewise/restore.py` owns the pure restore directive. `hooks/session_start.py` remains the Claude adapter. `hooks/codex_session_start.py` owns Codex input validation and `hookSpecificOutput.additionalContext`. `skills/reset/reset.py` remains the shared JSON CLI and delegates state work to the core. `hooks/hooks.json` remains Claude-specific. `hooks/codex-hooks.json` matches only Codex's four sources and runs `python "${PLUGIN_ROOT}/hooks/codex_session_start.py"`. `.codex-plugin/plugin.json` explicitly points at `./skills/` and `./hooks/codex-hooks.json`. `.agents/plugins/marketplace.json` defines `vibe-wise-local` with local `source.path: "./"`, relative to the repository root.

The core accepts normalized domain data, while adapters contain runtime event fields, environment names, and response envelopes. This keeps wire types out of learning and state logic per boundary discipline. One tuple defines the three-note invariant per model-the-domain. Preview and confirmation share one snapshot identity, preserving the existing convergent reset contract per make-operations-idempotent. The public surface stays small while hiding boundary lookup, legacy-directory handling, pause detection, and fingerprinting. Call chains remain adapter to core to result.

## Synthesis decision

This is an independent candidate for the parent synthesis. Its base is a shared domain core with explicit runtime adapters.

## Tradeoffs accepted

- We accept two tiny startup adapters and two hook configs in exchange for one learning policy and one state implementation.
- We accept a local Python package in exchange for removing the reset helper's import-path coupling to a hook script.
- We accept a Codex trust step because plugin hooks are skipped until the user reviews them.

## Alternatives considered

A separate Codex bundle would copy guides, reset rules, and tests. It hides runtime differences locally but exposes version coordination and state drift to maintainers, so it loses on interface depth. A single auto-detecting hook would reduce one file, but it would mix two wire contracts and command environments in the deepest shared module. A generated Codex package would avoid checked-in adapters but add a build step before local installation and make the installed artifact harder to audit.

## Open questions and risks

Should Codex skill names remain `$learn` and `$reset`, or should a namespace be retained if the client exposes one? Does Codex 0.149 reject any compatibility-manifest field before implementation begins? Windows CI should verify that the exact `python` hook command survives JSON parsing and paths with spaces.

## Next implementation step

Add contract tests for both adapters against the same restore and reset fixtures, then add the Codex manifest and marketplace files.
