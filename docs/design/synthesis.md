# Codex port design decision

Choose the separate Codex bundle from candidate B. The independent cross-judge also chose B. Candidate A's shared package introduces a core module, types, adapters, and import movement for two existing scripts. Those changes add risk to the original Claude installation. B isolates native packaging and host instructions while preserving the existing product.

| Criterion | A shared core | B separate bundle |
| --- | --- | --- |
| Runtime compatibility | 3 | 3 |
| Learning-state safety | 5 | 4 |
| Simple installation | 4 | 4 |
| Maintenance cost | 2 | 3 |
| Runnable verification | 4 | 4 |

The cross-judge scored B's installation at 3 pending a real install. The parent scores its planned install at 4 because the plugin path and marketplace commands match the installed CLI and official guide. Neither score proves runtime success.

Graft shared behavior fixtures from A so both bundles must preserve the same state selection, pause, and reset contracts. Keep B's installed-relative helper paths and ordinary-chat confirmation. Reject generic adapters and a build generator. The duplication is a known maintenance cost, constrained by tests.

Model the Domain retains the original three-note shape and existing reset outcomes. Laziness Protocol avoids new core types and runtime layers. Separate Before Serializing Shared State gives the implementation owner an isolated worktree. Build the Lever adds a rerunnable verifier. Prove It Works requires CLI and app-server discovery in addition to helper tests.

There is no unresolved design disagreement. Implementation must follow implementation-contract.md. The source checkout is on codex/vibe-wise-codex. The worker checkout is C:\Kuliah\Vibe Coding\vibe-wise-codex-work on codex/vibe-wise-implementation. No remote publication is part of delivery.
