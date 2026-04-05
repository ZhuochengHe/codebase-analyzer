# codebase-analyzer

A reusable skill for generating a structured, resumable analysis document for any codebase.

## Usage

Copy `SKILL.md` into your agent's `skills/` folder (e.g., `skills/codebase-analyzer.md`).
Then invoke it by asking: “analyze this repo” and providing a local path or GitHub URL.
It will create/update `<project-root>/.analysis.md` as the checkpointed output.