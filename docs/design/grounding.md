# VibeWise runtime grounding

The current package has two skill entry points, one read-only Python session hook, and one Python reset helper. Notes are three Markdown files selected within the nearest Git boundary. They hold a learner profile, evidenced progress with pending checkpoint stages, and an evidence-based project map.

The hook restores reading instructions only. Paused profiles stay inactive. Missing notes never activate learning. The reset helper previews the exact snapshot and requires its token before backing up originals and replacing the three notes. Reset is not a transaction across all three files. Failures preserve originals in the backup and report the path.

Codex CLI 0.149.0 is installed. Its plugin commands accept local marketplaces and expose JSON install/list results. Installed native plugins use `.codex-plugin/plugin.json`, explicit skills and hook paths, and optional interface metadata. The official plugin guide documents `PLUGIN_ROOT` with Claude-compatible aliases. SessionStart accepts `startup`, `resume`, `clear`, and `compact`; JSON additionalContext is supported. Non-managed hooks require user trust through `/hooks`.

The port must keep activation explicit, honor user skips and direct implementation, use available Codex file tools, use ordinary chat for required confirmations, and preserve learning-state boundaries. Windows commands use `python`. Existing Claude behavior should keep working.

Sources are the repository at commit 1135f4a, local CLI help, installed plugin manifests, and the official plugin and hook guides downloaded into this directory. The grounding agent traced the hook, reset helper, learning guides, and 37 existing tests without editing the repository.
