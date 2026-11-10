# MARL lecture set

Beamer/TikZ slides for a graduate course on multi-agent reinforcement learning, following
Albrecht, Christianos & Schäfer, *Multi-Agent Reinforcement Learning: Foundations and Modern Approaches* (MIT Press, 2024).

| File | Lecture | Book |
| --- | --- | --- |
| `lecture01.tex` | Introduction and models of multi-agent interaction | Ch. 1, 3 |
| `lecture02.tex` | Solution concepts I: best response, minimax, Nash | §4.1–4.5 |

All lectures load the shared preamble `marl-style.sty` (colours, TikZ styles, notation), so keep it in the same folder.

## Build

```
latexmk -pdf lecture01.tex
latexmk -pdf lecture02.tex
```

On Overleaf: upload the zip as a new project, then pick the lecture to compile under Menu → Main document.
