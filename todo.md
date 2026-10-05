# Codex port work

- [x] 1. `how` over the affected subsystem.
- [x] 2. `architect` for parallel design exploration. Skipping stays as `architect skipped: <reason>`; do not fold the design decision silently into implementation.
- [x] 3. Write the throughput checkpoint as four todo items.
  - [x] Blocking first steps. Confirm actual Codex plugin, marketplace, and hook contracts before implementation.
  - [x] Independent workstreams. Read-only grounding and design candidates have separate outputs. One implementation owner handles the coupled plugin.
  - [x] Shared mutable state. Give the implementation owner an isolated Git worktree. Main checkout owns this checklist and design decision only.
  - [x] Smallest safe decomposition. One worker preserves the shared notes and reset contracts across both runtimes.
- [x] 4. Delegate code-writing to a subagent using your configured feature model with a specific scope; review its diff yourself.
- [x] 5. Verify on the matching surface.
- [x] 6. Rebase into small, ordered commits; stack follow-ups.
  - skip: Deliver locally. No push, remote PR, or deployment requested.
- [x] 7. If the design is contested, `interrogate` before shipping.
  - skip: uncontested. Both judge and synthesis picked separate bundle B with grafted fixtures.
- [x] 8. Run Opening a PR.
  - skip: Deliver locally. Do not publish this conversion without a user request.

## Skill authoring

- [x] 1. Use the platform's skill-authoring guidance.
- [x] 2. Validate the skill: frontmatter has `name` and `description`, referenced files exist, cross-skill links resolve.
- [x] 3. Test cases if structural; skip if subjective.
- [x] 4. Run Opening a PR.
  - skip: Local delivery only.

## Design arena

- [x] Ground.
- [x] Frame.
- [x] Fan out.
- [x] Cross-judge.
- [x] Pick.
- [x] Graft.
- [x] Verify.

Done means a Codex plugin and local marketplace load through the real Codex CLI, learning and reset skills use supported tools, hook and reset tests pass, and installation instructions name tested commands. Live teaching quality remains a manual check.

The comparison rubric is runtime compatibility, preserved learning-state safety, simple installation, maintenance cost, and runnable verification. Each criterion is scored from 1 to 5.
