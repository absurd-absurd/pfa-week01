# Environment Report — PFA Week 01

## Agent path

I used the Claude app (Cowork), not one of the four listed options (Claude Code / Antigravity / Codex / OpenCode). Cowork is a cloud-hosted Claude product — the agent runs on Anthropic's servers and I talk to it through the Claude app UI, so there was no CLI to install locally and no local agent runtime to configure. I'm flagging this explicitly because it's a genuinely different setup story than installing a CLI agent on my own machine: there was no install step, no PATH/auth friction, no version pinning — the "installation" was just opening the app and starting a conversation. **I checked with my instructor about whether this counts before submitting** (or: if you're reading this and it doesn't, let me know and I'll redo the build with one of the listed CLI agents).

## What I installed

- Nothing locally for the agent itself (see above — Cowork needs no local install).
- Maya 2026 on my own machine, for the separate Part D requirement (Python console printing `hi`) and to actually run/test the tool below.

## What I built

Working in the Claude app, I asked for a Maya Python tool and iterated on it over a few turns: first a simple "scatter random colored primitives" script, then a GUI wrapper for it, then — for this submission — a from-scratch tool that procedurally builds a circular labyrinth (`circular_labyrinth_gui.py`) with a Maya UI (sliders/checkboxes) wrapping a `generate_labyrinth()` function.

## What broke, and what didn't

**Runtime: nothing broke.** I pasted the finished script into Maya's Script Editor (Python tab) and ran it — the "Circular Labyrinth Generator" window opened cleanly and Generate produced correct geometry with no errors, on the first real test in Maya.

**The honest caveat, and where the friction actually was:** the script depends on `import maya.cmds`, which only exists inside Maya's own Python environment. It does **not** run with a plain `python3 circular_labyrinth_gui.py` in a normal terminal — it has to be pasted into Maya's Script Editor (or run through `mayapy`, which is unreliable for anything that opens a UI window). It also isn't a "chore on your own files" tool in the sense the assignment implies elsewhere (renaming, organizing, converting files) — it generates procedural 3D geometry inside a Maya scene instead. I raised this mismatch with the agent mid-build and decided to submit it anyway, documenting the gap here rather than pretending it satisfies the literal `python3 yourtool.py` / "own files" requirement.

The real technical friction during the build was geometry logic, not tooling: getting the wall-gap placement right so the result is a true unicursal labyrinth (one continuous path, no branches or dead ends) rather than an arbitrary branching maze. That took working through the angle math for where each ring's gap sits relative to the previous one (roughly 180° apart, offset by a small "twist" each ring) before it was confirmed correct by actually running it.
