---
name: marl-lecture
description: Build, revise, or review a lecture in the MARL lecture set (Beamer + TikZ slides that follow the Albrecht, Christianos & Schäfer MARL book). Use when asked to write the next lecture, change a lecture, add derivations or diagrams, or check a deck.
---

# MARL lecture builder

This skill builds graduate lectures on multi-agent reinforcement learning as LaTeX Beamer decks with TikZ diagrams. The course follows Albrecht, Christianos & Schäfer, *Multi-Agent Reinforcement Learning: Foundations and Modern Approaches* (MIT Press, 2024), in book order.

## Files in the project folder

- `syllabus.md`: the 17-lecture plan. Each row gives book sections, load, the derivations to do, and a status. **Read it first.** Update the status column when a lecture is done.
- `marl-style.sty`: shared preamble. Every lecture starts with `\documentclass[aspectratio=169,11pt]{beamer}` and `\usepackage{marl-style}`. Put new shared colours, TikZ styles, or notation macros here, never in a single lecture.
- `lectureNN.tex`: one file per lecture, two-digit number (`lecture03.tex`).
- The book PDF, if the user has granted a folder containing it. Read the sections for the lecture before writing.

## Workflow for a new lecture

1. Read `syllabus.md` and the matching row. Read the previous lecture's `.tex` so the new one continues its notation and examples.
2. Read the book sections for the lecture (extract text from the PDF with `pdftotext -layout -f <first> -l <last>`). Note the book's definition numbers, equation numbers, figure numbers, and running examples. Cite them on the slides ("Definition 8", "book Eq. 4.18", "book Figure 4.4").
3. Check the "Material already drafted" table in `syllabus.md` and reuse those slides where they fit.
4. Plan 30–40 content slides. If the material does not fit, say so and propose a split. Do not shrink fonts to fit more in.
5. Write the deck (structure below), compile it, render it, look at every page, and fix problems (see "Check the result").
6. Update `syllabus.md`, then commit to git if the folder is a repository.

## Deck structure

1. Title frame, with "Book: Sections X–Y" under the title page.
2. "Today": numbered topics with section numbers on the right (`\hfill \S4.6`), then `\reading{...}`.
3. One `\section{}` per book section. `marl-style` inserts a divider page for each.
4. Content: definitions as `block`s titled with the book's number ("Definition 8 (Correlated equilibrium)"); claims with full proofs or derivations ending in `\hfill $\square$`; worked numeric examples; at least one diagram per section.
5. "Summary": one bullet per concept, then "Next lecture" naming the next book sections.
6. "Exercises": 5–7 problems. Mix proofs, by-hand computations, and one small coding task.
7. "References": the book first, then the primary sources cited on the slides.

## Style rules

- **Slides stay plain:** black text, default Beamer fonts, bullets, equations, `booktabs` tables. No coloured slide backgrounds, title graphics, or decorative boxes.
- **Diagrams can be rich:** TikZ diagrams may use colour, fills, gradients, vector fields, plots and many nodes. Use the palette in `marl-style.sty` (`cblue`, `corange`, `cgreen`, `cpurple`, `cred`, `cgray`) and its node styles (`b`, `bb`, `bo`, `bg`, `bp`, `br`, `note`). Colour must not be the only carrier of meaning: label the important things.
- **Slides read like lecture notes:** derive results step by step on the slide, not "it can be shown". Show the algebra a student would need to reproduce the result.
- **Notation follows the book:** agents $I$, actions $A_i$, joint action $a$, rewards $R_i$, transitions $T$, observation function $\mathcal{O}_i$ (calligraphic on slides to keep it apart from the set $O_i$), histories $h$, $\hh$ for full histories, expected return $U_i(\pi)$, best response $\BR_i$. Use the macros in `marl-style.sty`.
- **Vocabulary:** RL terms (agent, reward, policy, deterministic/probabilistic), with the game-theory synonym once when it first appears.
- Small fonts (`\small`, `\footnotesize`) are fine on dense derivation slides. Nothing smaller than `\scriptsize` except labels inside diagrams.
- **Facts:** use only claims from the book or from sources you checked. Never invent citations. Placeholders such as `[Instructor name]` stay as they are.

## TikZ pitfalls seen in this project

- Node names cannot contain a decimal point. `\foreach \x in {-4.6,...}` with `(n\x)` breaks. Use `[count=\k]` and name nodes `(n\k)`.
- Labels on short arrows collide with boxes. Leave at least 2 cm between boxes that carry edge labels, or move the label onto a longer segment.
- Wide diagrams overflow the 16:9 frame (about 14 cm usable). Set explicit coordinates and check the rendered page.
- Long equations in a half-width column overflow into the next column. Split them with `align*`.
- `\cellcolor` needs `colortbl`, which `marl-style.sty` loads.

## Check the result

1. Compile: `latexmk -pdf -interaction=nonstopmode -halt-on-error lectureNN.tex`. Fix every `!` error. Treat "Overfull \hbox" warnings over 10pt as layout problems.
2. Render every page (for example `pdftoppm -r 55 -png lectureNN.pdf page`), tile 9 pages per image, and look at all of them. Check for text past the frame edge or bottom, overlapping labels, unreadable diagrams, and text running across a column boundary.
3. Fix, recompile, and re-render the changed pages until they are clean.
4. Check the arithmetic in every worked example by recomputing it.
5. Clean build files (`latexmk -c`). PDFs stay out of git (`.gitignore`).

## Tooling

LaTeX may not be installed. Check with `which latexmk pdflatex`. If it is missing, install a TeX distribution into a conda environment, for example `conda install -c conda-forge tectonic` (then `tectonic lectureNN.tex`), or ask the user to allow the needed network hosts. Never change the user's system outside the project.
