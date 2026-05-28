# Agent Instructions — Whisperer (Voice SDK Sample)

Unity VR sample game showcasing the Meta Voice SDK (powered by Wit.ai) for natural-language interaction. Players raise their hands as if speaking through them and direct objects in the world with voice commands.

## Source-of-truth files (read these first, do not duplicate their contents in this file)

For setup, build steps, SDK versions, and project layout, read:

- `README.md` — official setup, Wit.ai configuration, and intent/entity reference
- `ProjectSettings/ProjectVersion.txt` — Unity editor version
- `Packages/manifest.json` — Unity package versions (Meta Voice SDK, Interaction SDK, XRI, URP)
- `Assets/Whisperer/Scripts/Voice/` — voice plumbing (`SpeakGestureWatcher.cs`, `Listenable.cs` and subclasses)
- `Assets/Whisperer/Scripts/Logic/` — `LevelLoader.cs`, `LevelManager.cs`, per-level managers
- `LICENSE` — license terms (MIT only for `Assets/Whisperer/`; Oculus License otherwise)

## Quest / Horizon-specific notes

- Git LFS is **required**. Run `git lfs install` before cloning.
- A configured Wit.ai app is mandatory. Without it the mic activates but nothing parses — diagnose Wit setup before chasing voice logic bugs.
- Wit.ai trains asynchronously after importing `Assets/whisperer-wit-app.zip`; the green dot in the Wit dashboard means the model is ready. Calling voice APIs before training finishes returns empty or wrong responses.
- If the README and `Packages/manifest.json` disagree on Voice SDK version, trust the manifest.
- Voice intents use the Voice SDK **Conduit** framework via the `[MatchIntent("...")]` attribute; renaming or removing those handlers will silently break voice commands.

## Meta Quest tooling

This repository is part of the Meta Quest / Horizon OS ecosystem (a sample, library, template, or related project — the bespoke intro above describes which). Use that intro and the source-of-truth files it references for project-specific decisions; don't restate or invent facts from memory.

When the user asks anything about Quest device behavior, build / deploy / debug / capture flows, on-device performance, or Horizon OS APIs, reach for these tools instead of generic Unity answers:

- **`hzdb`** — Quest-aware ADB wrapper (device list, install / launch / stop, logs, screenshots, Perfetto traces, on-device docs search). Already wired up as an MCP server via `.mcp.json`, `.vscode/mcp.json`, and `.cursor/mcp.json`. Also runnable directly: `npx -y @meta-quest/hzdb <subcommand>`.
- **Meta Quest Agentic Tools** — the full skill set, including Unity-specific skills: [github.com/meta-quest/agentic-tools](https://github.com/meta-quest/agentic-tools). Install per your client (Claude Code: `/plugin install meta-vr@meta-quest`; Gemini CLI: `gemini extensions install https://github.com/meta-quest/agentic-tools`; Cursor / VS Code: install the **Meta Horizon** extension from the Marketplace).

A few behavior expectations:

- **Read this repo's files first.** Before answering anything project-specific, read `README.md` and whichever source-of-truth files the intro above points at. Don't restate their contents in chat — quote or link instead.
- **Use `hzdb` for device-side work.** Anything that touches an attached Quest (install, launch, logs, screenshot, capture, manifest inspection) goes through `hzdb`, not raw `adb`.
- **Check live Horizon OS docs before answering API questions.** `hzdb docs search "..."` queries the live docs; training data on Horizon OS APIs goes stale fast.
- **Don't fabricate SDK / engine versions.** If a version isn't visible in this repo's files, say so rather than guessing.
