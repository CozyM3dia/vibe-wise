# Candidate B. Separate Codex bundle

## Problem

Codex needs native learning guides, installation, and session restoration. Preserve the existing Claude package and add a self-contained `codex/` bundle. The three Markdown notes, read-only SessionStart hook, and snapshot-confirmed reset remain the contract. See [grounding.md](grounding.md).

## Usage (caller's view)

1. Run `codex plugin marketplace add "C:\Kuliah\Vibe Coding\vibe-wise"`. Install VibeWise from the resulting marketplace, then review and trust its hook through `/hooks`.
2. Select VibeWise Learn from Codex's skill picker in the target project. Codex loads adjacent guides and asks only unanswered onboarding questions. Subsequent sessions restore pending checkpoints without treating restart as approval.
3. Select Reset learning. Codex previews the exact notes and backup destination, then requests explicit confirmation in ordinary chat. Only that answer permits applying the preview token. Changed notes require another preview and confirmation.

The Reset guide resolves its sibling helper to an absolute installed path. Ordinary shell calls must not assume the hook-only `PLUGIN_ROOT` environment variable exists.

```powershell
python "<installed bundle>/skills/reset/reset.py" --cwd "C:\Projects\notes"
python "<installed bundle>/skills/reset/reset.py" --cwd "C:\Projects\notes" --confirm "<preview token>"
```

## Shape

These are sketch signatures for the existing behavior, not a new framework. Learner state remains `.vibe-wise/profile.md`, `progress.md`, and `project-map.md`; legacy `.sensible-vibes` stays supported in place.

```python
NoteName = Literal['profile.md', 'progress.md', 'project-map.md']
NoteSnapshot = dict[NoteName, bytes]
SnapshotToken = str  # Digest binds state path, file presence, and content.
NoNotes = {status: 'no_notes', cwd: str}
Preview = {status: 'preview', project: str, state: str, files: list[NoteName], backup_parent: str, confirmation: SnapshotToken}
ResetDone = {status: 'reset', project: str, state: str, backup: str}
ResetError = {status: 'error', message: str}
def state_directory(cwd: Path) -> Path | None: ...
def profile_is_active(path: Path) -> bool: ...
def restore(payload: object) -> dict | None: ...
def snapshot(cwd: Path) -> tuple[Path | None, NoteSnapshot, SnapshotToken | None]: ...
def reset(cwd: Path, confirmation: SnapshotToken | None = None) -> NoNotes | Preview | ResetDone: ...
```

The hook owns event parsing and emits `hookSpecificOutput.additionalContext` or silence. It validates absolute cwd and active profile, then supplies Codex-native reading instructions. It never emits learner content, writes notes, parses transcripts, or activates paused learning. Match `startup|resume|clear|compact`. Reset owns snapshot comparison, backups, and replacement; its CLI maps errors to `ResetError` and exit status 1. The guide owns human consent, since a token alone does not prove consent. Preserve nearest-state lookup, Git/worktree boundaries, and symlink refusal.

| File ownership | Responsibility |
| --- | --- |
| Existing root `.claude-plugin/`, `skills/`, `hooks/` | Original Claude package remains independently owned. |
| `.agents/plugins/marketplace.json` | Codex catalog with `source: {source: "local", path: "./codex"}`. |
| `codex/.codex-plugin/plugin.json` | Explicit `skills: "./skills/"` and `hooks: "./hooks/hooks.json"`. |
| `codex/skills/learn/` | Codex-only Learn, behavior, onboarding, and state templates. |
| `codex/skills/reset/` | Codex reset guide and bundled existing reset helper. |
| `codex/hooks/` | Bundled hook and config using `python`, quoted `${PLUGIN_ROOT}` path, and five-second timeout. |
| `codex/README.md`, root `README.md` | Native setup instructions and a short root link. |
| `tests/` | Existing behavior fixtures against both bundles, plus Codex manifest/path/prompt checks. |

Boundary Discipline places host-specific tools and event handling at the bundle boundary. Minimize Reader Load keeps installed references inside one bundle. Two skills hide restoration and reset mechanics without a host selector, forwarding layers, or generation step. Copy the small Python helpers; adapt hook prompt text and skill instructions. Equivalent behavioral tests constrain drift.

## Synthesis decision and tradeoffs

Candidate B proposes the separate bundle as the base; synthesis belongs to the orchestrator. Accept duplicated guides and small helpers for standalone installation and independent runtime evolution. Both hosts share note semantics to allow switching hosts, but simultaneous edits remain unsupported. Existing reset checks catch observed changes; they do not make replacement transactional. Partial failures report the backup and stop onboarding.

## Alternatives considered

A shared dual-runtime package hides duplication but exposes host conditions throughout guides and couples releases. A generated Codex distribution reduces source duplication but adds generation and freshness contracts for two skills and two helpers. A skills-only install leaves restoration wiring and trust setup to the user and misses the plugin experience.

## Implementation reconciliation and risks

No implementation or deviations yet. Will Codex 0.149 discover this bundle and execute its quoted Windows hook command? Verify installation, hook trust, active/paused restoration, unchanged note bytes, and reset preview from the installed copy. Run shared behavioral fixtures for both bundles to catch semantic drift.

## Next implementation step

Create the Codex bundle and catalog, then verify installed restoration before claiming support.
