# explain

An agent skill that turns an explanation into a narrated slide deck — one
self-contained HTML file that plays itself. It speaks a step, lights up the code
lines or diagram nodes that step is about, and advances when the audio ends.

Audio, syntax highlighting and diagrams are all inlined. The output is a single
file you can open over `file://`, email, or drop on a share — no server, no
network, no player to install.

Speech is synthesised on your machine. Your code never leaves it.

## Install

Clone it into wherever your coding agent keeps skills:

```bash
git clone https://github.com/<you>/explain ~/.claude/skills/explain
```

Other agents use other directories — `.agents/skills/`, `.cursor/skills/`,
`~/.config/<agent>/skills/`. The skill is a plain folder with a `SKILL.md`; drop
it in and the agent picks it up. Then ask your agent to explain something:

```
/explain how does the ingestion pipeline choose a parser?
```

**Requirements: Node 18+ and npm.** That is the whole list. Everything else is
bootstrapped on first use.

## First run

The first build installs its dependencies into the skill's own folder and
downloads a speech model, which takes a minute or two. After that, only clips
whose narration actually changed are re-rendered, so editing a sentence rebuilds
in well under a second.

## Speech backends

The same model ([Kokoro](https://huggingface.co/hexgrad/Kokoro-82M), 82M
parameters, 28 voices) runs through one of two engines, chosen automatically:

| | When it's used | Speed | Cost |
|---|---|---|---|
| **ONNX** (default) | everywhere — macOS, Linux, Windows | ~2.5× realtime | one-time ~310 MB runtime + 92 MB model |
| **MLX** (optional) | Apple Silicon, if a Python with `mlx-audio` is already present | ~17× realtime | nothing extra |

The MLX path is a happy accident, not a dependency: the skill probes for it and
silently falls back. Nothing needs to be installed for it, and nothing breaks
without it. Force either with `EXPLAIN_TTS=onnx` or `EXPLAIN_TTS=mlx`.

## Usage

The agent writes a `deck.json` and runs:

```bash
node build.mjs path/to/deck.json          # -> path/to/<folder-name>.html
node build.mjs path/to/deck.json --folder # multi-file form, for very long decks
node build.mjs path/to/deck.json --no-open
```

See [SKILL.md](SKILL.md) for the deck format and the guidance the agent follows.

## Controls

`space` pause · `←` `→` step · `↑` `↓` slide · `[` `]` speed · click any slide in
the sidebar.

## License

MIT. Kokoro is Apache 2.0.
