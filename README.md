# MMAG — Mixed Memory-Augmented Generation

> An OpenClaw skill implementing the MMAG pattern: a five-layer cognitive memory architecture for AI agents.

[**Skill on ClawHub**](https://clawhub.ai/J0ker98/mmag)

MMAG organizes agent memory across five interacting types inspired by cognitive psychology, enabling agents to move beyond single-session, reactive interactions toward **personalized, context-rich, and time-aware dialogue**.

---

## Memory Layers

| Layer | Cognitive Analogy | What it stores |
|---|---|---|
| `conversational/` | Discourse memory | Dialogue threads, turn history |
| `long-term/` | Biographical / semantic memory | User profile, preferences, traits |
| `episodic/` | Episodic memory | Timestamped events, reminders, habits |
| `sensory/` | Situational awareness | Location, weather, time of day |
| `working/` | Working memory | Current session scratchpad (ephemeral) |

---

## Install

```bash
# Via OpenClaw (published on https://clawhub.ai/J0ker98/mmag)
claw install mmag

# Or clone and use directly
git clone git@github.com:J0ker98/mmag-skill.git
bash ~/.openclaw/skills/mmag/init.sh
```

---

## Quick Start

```bash
# 1. Initialize memory structure
bash init.sh

# 2. Store memories
bash store.sh long-term "Prefers concise answers. Researcher." --label preferences
bash store.sh episodic "Team meeting on 2026-03-10 at 14:00."
bash store.sh sensory "Location: Milan. Weather: rainy. Time: morning."
bash store.sh conversational "User asked about their Tokyo trip."
bash store.sh working "Current goal: draft project summary."

# 3. Build LLM context
bash context.sh

# 4. End of session
bash prune.sh       # archive working → episodic
bash snapshot.sh    # save compressed backup
```

---

## Commands

| Script | Usage | Description |
|---|---|---|
| `init.sh` | `bash init.sh` | Create the 5-layer memory directory |
| `store.sh` | `bash store.sh <layer> "<text>" [--label <name>]` | Append a timestamped entry |
| `retrieve.sh` | `bash retrieve.sh <layer\|all> [query] [--no-redact]` | Search by layer and keyword |
| `context.sh` | `bash context.sh [--max-chars N] [--no-redact]` | Build prioritized LLM prompt context |
| `prune.sh` | `bash prune.sh` | Archive working memory, clear scratchpad |
| `snapshot.sh` | `bash snapshot.sh` | Full compressed snapshot of all layers |
| `stats.sh` | `bash stats.sh` | Per-layer file count, size, last entry |

---

## Context Priority Order

`context.sh` assembles layers in MMAG precedence order:

1. **Long-Term User Profile** — injected as system-level context (highest priority)
2. **Episodic Memory** — upcoming events and reminders
3. **Sensory Context** — environmental grounding
4. **Conversational History** — recent dialogue turns
5. **Working Memory** — current session scratchpad

---

## Suggested Agent Heartbeat

```markdown
## MMAG Memory
- Session start: bash store.sh working "Current task: <goal>"
- During:        bash store.sh conversational "<key exchange>"
- Session end:   bash prune.sh
- Weekly:        bash snapshot.sh
```

## Security Notes

- Prefer `MMAG_KEY_FILE` over exporting `MMAG_KEY` in shell environments.
- `retrieve.sh` and `context.sh` redact obvious secrets by default; use `--no-redact` only for trusted local debugging.
- Runtime dependencies: `bash`, `openssl`, `find`, `sed`, `grep`, `tar`, `du`.

---

## Based On

> Zeppieri, S. *Mixed Memory-Augmented Generation (MMAG)* 2025.
> [https://arxiv.org/abs/2512.01710](https://arxiv.org/abs/2512.01710)

MMAG draws on the cognitive psychology taxonomy of episodic, semantic, procedural, and working memory, mapping each to concrete technical components for LLM-based agents.

---

**License:** MIT
