# Beyond Dynamic Programming — Workshop Slides

An interactive, technically advanced workshop on dynamic programming, built on the
[beamer](https://ctan.org/pkg/beamer) [metropolis theme](https://github.com/matze/mtheme).
The deck takes an audience of strong programmers (competitive-programming and research
backgrounds) from the state-space view of DP through case studies, speed-up techniques, and
Bellman's equations for Markov decision processes.

The slides are designed to be **discursive**: every few slides contain an audience-engagement
element (poll, vote, predict, discuss, or pair), and the case studies ask the room to judge
candidate solutions before the answer is revealed.


# Contents

| File | Purpose |
|------|---------|
| `presentation.tex` | Main file to compile. Sets the title and includes the five content files. |
| `content-01-foundations.tex` | Part 1: DP from first principles (state spaces, the DAG view, the recipe), motivated by edit distance. |
| `content-02-case-studies.tex` | Part 2: three LeetCode case studies with candidate solutions to vote on, including two false positives. |
| `content-03-going-faster.tex` | Part 3: two speed-ups — the convex hull trick and a broken-profile bitmask DP. |
| `content-04-beyond.tex` | Part 4: MDPs, the Bellman equations, value/policy iteration, and asynchronous DP. |
| `content-05-closing.tex` | Part 5: semirings, DP in linear optimisation, the summary, and the appendix. |
| `setup-workshop.tex` | Workshop-specific macros (the `\audience` engagement box, state-space TikZ styles). |
| `setup-*.tex` | Theme, colour, maths, and plot setup inherited from the template. |
| `WORKSHOP_GUIDE.md` | Presenter's guide: how each interaction type runs, and a per-slide script with expected answers. |

The deck has 118 numbered slides plus section dividers and a short appendix (references
and a backup slide). Each slide is a single page: questions sit on their own slides with the
answer on the next one, and spoken material lives in `WORKSHOP_GUIDE.md`. The earlier, denser
version of the deck is kept as `presentation_audience.pdf`. Suggested running time is about
2.5 to 3 hours with discussion.


# Structure of the workshop

1. **Dynamic Programming from First Principles.** Introduces the *state space* of a problem and
   shows that when it forms a directed acyclic graph (DAG), the problem is solved by dynamic
   programming. Presents a four-step strategy — decompose the problem into a state space, state
   the base case and inductive step, evaluate bottom-up, and reconstruct the solution if needed —
   and reads off complexity as number of states times cost per state. Fully motivated by a single
   example (edit distance).
2. **Is It Dynamic Programming? Case Studies.** Problems that are and are not best solved by DP,
   with suggested solutions the audience judges:
   - *Split Array Largest Sum* — a false positive (DP is correct but inefficient);
   - *Maximum weight independent set on a tree* — a genuine DP;
   - *Minimum Initial Energy to Finish Tasks* — a false positive (no compact sufficient state).
3. **Going Faster.** Two techniques: changing the recurrence (the convex hull trick) and changing
   the compute order (a DP-plus-bitmask / broken-profile approach).
4. **Beyond Programming Problems.** Reinforcement learning as a Markov decision process, solved via
   the Bellman equations with dynamic programming, and the more advanced topic of asynchronous
   dynamic programming, which relaxes the assumptions of the classical techniques.
5. **Other Places, and What We Learned.** A short linear-optimisation example solved by DP, and a
   closing summary of the workshop.

Throughout, every solution to a DP problem is presented as a **recurrence**, in mathematical form,
never as code.


# Requirements

- A LaTeX installation including the [metropolis theme](https://ctan.org/pkg/beamertheme-metropolis),
  which ships with popular distributions (TeX Live, MiKTeX).
- Compile with **`lualatex`**; the [Fira fonts](https://github.com/mozilla/Fira) are the theme's
  default. Install them on your machine if you want to use them.


# Building

```bash
lualatex presentation.tex   # run twice so the outline and slide numbering settle
```

If you prefer a container with everything preinstalled:

```bash
docker run --rm -v "$PWD":/work -w /work texlive/texlive:latest \
  sh -c "lualatex presentation.tex && lualatex presentation.tex"
```


# Customising

- **Title / author.** Edit `\title`, `\author`, `\institute`, and `\date` in `presentation.tex`.
- **Logo.** Uncomment the `\titlegraphic{...}` line in `presentation.tex` and point it at your
  institution's logo.
- **Colours.** Edit the four colour definitions in [`setup-colors.tex`](setup-colors.tex):

  ```latex
  \definecolor{accent}{HTML}{7EBDC2}    % progress bar and \alert{text}
  \definecolor{bgcolor}{HTML}{FCFCFF}   % slide background
  \definecolor{bgcolorAlt}{HTML}{ECF1FC}% title and box background
  \definecolor{fgcolor}{HTML}{222244}   % text colour
  ```
- **Question boxes.** Each question for the room is an `\audience[<title>]{<question>}` box defined
  in `setup-workshop.tex`, on a slide of its own. The title is optional and should name the topic.


# Notes

- The remarks about the LeetCode problems are from memory of their statements; verify the current
  statements and constraints before presenting if you intend to quote them.
- The recurrences and counterexamples in the deck were checked against brute force before the deck
  was built.
- The `mkbeamerpresentation.sh` helper from the original template copies only `presentation.tex`
  and the `setup-*.tex` files; it does not pick up the `content-*.tex` files added here.


# License

See [`LICENSE`](LICENSE). The template this is based on is a clean-and-simple beamer presentation
template using the metropolis theme.
