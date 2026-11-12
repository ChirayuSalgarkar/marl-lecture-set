# MARL lecture set

Beamer/TikZ slides for self-contained 75-minute guest lectures on multi-agent reinforcement learning, following
Albrecht, Christianos & Schäfer, *Multi-Agent Reinforcement Learning: Foundations and Modern Approaches* (MIT Press, 2024).

| File | Lecture | Book |
| --- | --- | --- |
| `lecture01.tex` | Introduction and models of multi-agent interaction | Ch. 1, 3 |
| `lecture02.tex` | Solution concepts I: best response, minimax, Nash | §4.1–4.5 |

Speaker scripts: `lecture01-script.md` is a full word-for-word script for Lecture 1, one heading per slide, matching the slide counter.

All lectures load the shared preamble `marl-style.sty` (colours, TikZ styles, notation), so keep it in the same folder.

## Editing diagrams

Every diagram is its own file in `figures/lectureNN/`, named after its slide (for example `figures/lecture01/many-agents.tex`). Each holds one `tikzpicture` and is pulled into the deck with `\input{...}`. Edit the file and recompile the lecture. The four application pictures (`app-warehouse.tex`, `app-games.tex`, `app-driving.tex`, `app-trading.tex`) appear on two slides each, so they hold only drawing commands without the `tikzpicture` wrapper. The robot, person, car and apple icons are defined once in `marl-style.sty`.

## Build

```
latexmk -pdf lecture01.tex
latexmk -pdf lecture02.tex
```

On Overleaf: upload the zip as a new project, then pick the lecture to compile under Menu → Main document.

## Iterating in Claude Science

1. Unzip this folder somewhere permanent, for example `~/teaching/marl-lecture-set`. Put the book PDF in it, or in another folder you will grant. The PDF is not in git (see `.gitignore`).
2. In Claude Science, create a project ("MARL lectures") and paste `claude-science/project-instructions.md` into its custom instructions.
3. Add the skill: **Settings → Skills → Add skill → Upload a skill**, and choose `marl-lecture-skill.zip` or the `skills/marl-lecture` folder. If this repo is on GitHub, use **Import from GitHub** instead; **Check for updates** then picks up edits.
4. Start a session in the project and grant the folder (type its path or use `@`). For example: "Build Lecture 3 from the syllabus."

Files:

- `syllabus.md`: the 17-lecture plan in book order, with a status column.
- `skills/marl-lecture/SKILL.md`: the workflow and style rules Claude follows.
- `claude-science/project-instructions.md`: text for the project's custom instructions.
