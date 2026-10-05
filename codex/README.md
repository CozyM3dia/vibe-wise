# VibeLearn for Codex

Mindful pair programming that keeps you in the driver's seat.

You lead the design; Codex gives feedback, explains concepts, asks follow-up questions, and writes the agreed code.

## Requirements

- [Codex CLI](https://github.com/openai/codex)
- Python 3 (`python` on Windows, `python3` on Unix) with no third-party Python dependencies needed.

## Installation

### 1. Add the marketplace

From this repository (or pointing to its root directory):

```sh
codex plugin marketplace add .
```

Or directly via GitHub shorthand:

```sh
codex plugin marketplace add CozyM3dia/vibe-wise
```

To verify the marketplace is registered:

```sh
codex plugin marketplace list
```

### 2. Install the plugin

Install `vibe-learn` from the marketplace:

```sh
codex plugin add vibe-learn@vibe-learn
```

Verify installation:

```sh
codex plugin list
```

### 3. Review and trust the hook

VibeLearn registers a `SessionStart` hook (`codex/hooks/hooks.json`) that restores learning context at session start and after compaction. In Codex, hooks from newly installed plugins remain untrusted until user review.

Open `/hooks` in Codex, inspect the VibeLearn hook:

```text
python "${PLUGIN_ROOT}/hooks/session_start.py"
```

Approve and trust the hook for your workspace.

## Usage

### Start learning

In your project directory, launch Codex and invoke the Learn skill:

```text
$learn
```

Onboarding guides you through setting your goals and checkpoint preferences one question at a time. Then describe what you want to build; Codex will invite your design approach before writing any code.

### Reset learning notes

To back up existing learning notes and restart onboarding from scratch:

```text
$reset
```

The command generates a preview with a snapshot confirmation token. Confirm in chat to create a timestamped backup in `.vibe-learn/backups/` and reset only learning notes. Your project code and Git history are never modified.
