---
name: marl-lecture
description: Build, revise, or review a lecture in the MARL lecture set (picture-first Beamer + TikZ slides that follow the Albrecht, Christianos & Schäfer MARL book). Use when asked to write the next lecture, change a lecture, add or edit diagrams, or check a deck.
---

# MARL lecture builder

This skill builds lectures on multi-agent reinforcement learning as LaTeX Beamer decks that explain with TikZ pictures. The course follows Albrecht, Christianos & Schäfer, *Multi-Agent Reinforcement Learning: Foundations and Modern Approaches* (MIT Press, 2024), in book order.

## Files in the project folder

- `syllabus.md`: the 17-lecture plan. Each row gives book sections, load, the derivations to do, and a status. **Read it first.** Update the status column when a lecture is done.
- `marl-style.sty`: shared preamble. Every lecture starts with `\documentclass[aspectratio=169,11pt]{beamer}` and `\usepackage{marl-style}`. Put new shared colours, TikZ styles, or notation macros here, never in a single lecture.
- `lectureNN.tex`: one file per lecture, two-digit number (`lecture03.tex`).
- `figures/lectureNN/`: one `.tex` file per diagram, holding a single `tikzpicture` and named after its slide (`figures/lecture01/many-agents.tex`). The lecture pulls it in with `\input{figures/lectureNN/<name>.tex}`. Pictures reused on several slides are drawing-only files (no `tikzpicture` wrapper, e.g. `app-warehouse.tex`) wrapped in a macro in the lecture preamble. End every figure file with `%` so the line break adds no space.
- The book PDF, if the user has granted a folder containing it. Read the sections for the lecture before writing.

## Audience and level

- Each deck is a **self-contained 75-minute guest lecture**, not part of a course. Never refer to other lectures, "this course", a syllabus, homework, or reading assignments. Point onward to book sections instead ("book \\S6.3"). End with a Summary, a "Questions?" slide with the book reference, and References. No exercises slide.
- Teach it like an undergraduate lecture, even though the students are PhD students who know single-agent RL. Assume no game theory.
- Keep it simple: one idea per slide.
- **Explain with pictures.** Each slide is built around a diagram, with few or no words: short labels, a one-line takeaway, and a small grey book reference (`{\scriptsize\color{cgray} Book \S1.3}`). No paragraphs, and no bullet lists longer than about three short items.
- Open a topic with a concrete application or example before the abstraction. Lecture 1 opens with the book's applications (\S1.3) and the MARL loop (Figure 1.3), following Albrecht's own lectures.
- Use overlays (`\onslide<2->`, `\only<n>`) to build a diagram step by step instead of explaining it in text.
- Put formal definitions and derivations after the picture that motivates them, and keep them short. Leave longer derivations to the exercises or the book.

## Workflow for a new lecture

1. Read `syllabus.md` and the matching row. Read the previous lecture's `.tex` so the new one continues its notation and examples.
2. Read the book sections for the lecture (extract text from the PDF with `pdftotext -layout -f <first> -l <last>`). Note the book's definition numbers, equation numbers, figure numbers, and running examples. Cite them on the slides ("Definition 8", "book Eq. 4.18", "book Figure 4.4").
3. Check the "Material already drafted" table in `syllabus.md` and reuse those slides where they fit.
4. Plan 30–40 slides (picture slides are quick; overlay steps do not count separately). If the material does not fit, say so and propose a split. Do not shrink fonts to fit more in.
5. Write the deck (structure below), compile it, render it, look at every page, and fix problems (see "Check the result").
6. Update `syllabus.md`, then commit to git if the folder is a repository.

## Deck structure

1. Title frame, with "Book: Sections X–Y" under the title page.
2. "Today": the talk's topics as a picture (numbered steps with book sections), no reading line.
3. One `\section{}` per book section. `marl-style` inserts a divider page for each.
4. Content: mostly picture slides. Definitions as short `block`s titled with the book's number ("Definition 8 (Correlated equilibrium)"), each after a picture that motivates it. Derivations only where needed, short, ending in `\hfill $\square$`. Worked numeric examples drawn as pictures where possible.
5. "Summary": one picture or one bullet per concept, with pointers to the book chapters that go further.
6. "Questions?" closing slide with the book reference.
7. "References": the book first, then the primary sources cited on the slides.

## Style rules

- **Slide frame stays plain:** black text, default Beamer fonts, no coloured slide backgrounds or title graphics. The colour lives in the diagrams.
- **Diagrams carry the lecture:** TikZ diagrams may use colour, fills, plots, icons and many nodes. Use the palette in `marl-style.sty` (`cblue`, `corange`, `cgreen`, `cpurple`, `cred`, `cgray`, `cyellow`), its node styles (`b`, `bb`, `bo`, `bg`, `bp`, `br`, `note`) and its pictograms (`\pic{robot}`, `\pic{person}`, `\pic{car}`, `\pic{apple}`). Colour must not be the only carrier of meaning: label the important things. In application slides, use the same colours for the same roles: green = observes, orange = acts, purple = reward.
- **Notation follows the book:** agents $I$, actions $A_i$, joint action $a$, rewards $R_i$, transitions $T$, observation function $\mathcal{O}_i$ (calligraphic on slides to keep it apart from the set $O_i$), histories $h$, $\hh$ for full histories, expected return $U_i(\pi)$, best response $\BR_i$. Use the macros in `marl-style.sty`.
- **Vocabulary:** RL terms (agent, reward, policy, deterministic/probabilistic), with the game-theory synonym once when it first appears.
- Nothing smaller than `\scriptsize` except labels inside diagrams.
- **Facts:** use only claims from the book or from sources you checked. Never invent citations. When a picture simplifies a book example, keep the book's mechanism (e.g. in the credit-assignment example the agent contributes nothing because its level is too low, not because it is far away). Placeholders such as `[Instructor name]` stay as they are.

## TikZ pitfalls seen in this project

- Node names cannot contain a decimal point. `\foreach \x in {-4.6,...}` with `(n\x)` breaks. Use `[count=\k]` and name nodes `(n\k)`.
- Labels on short arrows collide with boxes. Leave at least 2 cm between boxes that carry edge labels, or move the label onto a longer segment.
- Wide diagrams overflow the 16:9 frame (about 14 cm usable). Set explicit coordinates and check the rendered page.
- Long equations in a half-width column overflow into the next column. Split them with `align*`.
- `\cellcolor` needs `colortbl`, which `marl-style.sty` loads.
- `\draw (0,0) grid (8,8)` inside a scaled picture (`x=0.6cm`) still uses a 1 cm step, giving the wrong number of cells. Write `grid[step=1]`.
- A `\foreach` list whose items contain `\\` or formatting breaks with "Missing \endcsname". Write those nodes out one by one.

## Check the result

1. Compile: `latexmk -pdf -interaction=nonstopmode -halt-on-error lectureNN.tex`. Fix every `!` error. Treat "Overfull \hbox" warnings over 10pt as layout problems.
2. Count "Overfull \vbox" warnings too: they mean content runs into the footline.
3. Render every page (for example `pdftoppm -r 55 -png lectureNN.pdf page`), tile 9 pages per image, and look at all of them. Check for text past the frame edge or bottom, overlapping labels, unreadable diagrams, and text running across a column boundary.
4. Fix, recompile, and re-render the changed pages until they are clean. After moving code between files, compare the old and new renders page by page.
5. Check the arithmetic in every worked example by recomputing it.
6. Clean build files (`latexmk -c`). PDFs stay out of git (`.gitignore`).

## Tooling

LaTeX may not be installed. Check with `which latexmk pdflatex`. If it is missing, install a TeX distribution into a conda environment, for example `conda install -c conda-forge tectonic` (then `tectonic lectureNN.tex`), or ask the user to allow the needed network hosts. Never change the user's system outside the project.
