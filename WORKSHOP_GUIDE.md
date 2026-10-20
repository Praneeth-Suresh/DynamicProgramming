# Beyond Dynamic Programming: Presenter's Guide

This guide accompanies `presentation.tex` / `presentation.pdf`. It explains how the audience
interaction is built into the slides and what to say at each slide.

- **Slide numbers below are the numbers printed in the slide footer.** The title slide and the
  five section-divider slides are unnumbered. The Outline is slide 1.
- Several slides have two PDF pages (an *overlay*): the question first, the answer on the next
  keypress. The footer number stays the same on both pages. Overlays are marked **[reveal]**.
- Total: 71 numbered slides + 6 unnumbered ones, plus a 3-slide appendix (references, backup).
- Suggested time: about 2.5 to 3 hours with discussion. Rough budget: Part 1 35 min, Part 2 40 min,
  Part 3 35 min, Part 4 45 min, Part 5 15 min.

## How the interaction works

Every interaction slide contains a teal box titled **Audience | \<type\>**. There are five types:

| Type | What the audience does | How to run it |
|------|------------------------|---------------|
| **Poll** | Show of hands on a multiple-choice question | Read the options, count hands, then reveal the answer |
| **Vote** | Decide correct / incorrect (or choose between options) for each candidate | Ask for hands for each candidate in turn; make one person who voted *incorrect* produce a counterexample before you reveal |
| **Predict** | Commit to a prediction before seeing the answer | Give 30 to 60 seconds of silence, take two or three guesses, then reveal |
| **Discuss** | Open question with a worked answer behind the reveal | Take answers from the room first; only reveal when the discussion stalls |
| **Pair** | Talk to a neighbour for a few minutes | Use a timer (3 min), then collect two or three reports |

**Rule enforced by the deck:** there are never more than 4 consecutive slides without an
interaction (section dividers and the Outline count as slides). Interaction slides are 2, 6, 10,
13, 17, 20, 22, 25, 27, 30, 33, 37, 40, 43, 47, 52, 54, 58, 61, 63, 66 and 69.

**General tips**

- Do not reveal early. On the case-study slides the whole point is that people disagree.
- Wrong answers are valuable here: the audience is expert, so many people will confidently vote for a
  plausible but incorrect proposal. Ask *why* before you give the counterexample.
- If the room is quiet, call on the two groups explicitly: "the competitive programmers" and
  "the people from research". They often disagree about what counts as an efficient solution.

## Building the slides

```bash
lualatex presentation.tex   # run twice so the outline and numbering settle
```

The deck uses `lualatex` and the Metropolis theme, as the template README describes. Replace `Your Name`
and `Your Affiliation` in `presentation.tex`. The placeholder university logo from the template is
commented out (`\titlegraphic`).

---

# Part 0: Opening

**Title slide.** Welcome the room. State the contract: this is a workshop, not a lecture, and the
goal is that *everyone* leaves with at least one idea about dynamic programming they did not have before.

**Slide 1: Outline.** Read the five parts. Say that Part 1 is brief and is there to fix notation
(the state-space view), and that the most unusual material is in Parts 3 and 4.

**Slide 2: Calibration (Poll).** Three questions by show of hands: convex hull trick / Li Chao tree;
Bellman optimality from memory; "is Dijkstra a DP?".
*Say:* "This tells me where to spend time. If many hands go up for the first, I will go quicker on slide 36.
If few go up for the second, I will slow down in Part 4." Note the hands for question 3: the answer
(yes, it is value iteration with a clever schedule) is revealed in slides 31 and 57.

# Part 1: Dynamic Programming from First Principles

**Slide 3: A motivating problem: minimum edit script.** *Say:* "Think `git diff`. We have two token
sequences and want the cheapest sequence of insertions, deletions and substitutions." Stress the last
paragraph: the number of scripts is exponential, so the question is how to organise the search. Do not
mention the recurrence yet.

**Slide 4: State spaces.** *Say:* "A state space is a graph whose paths are solutions. A state is
a summary of a partial solution." Dwell on **sufficiency**: the future must depend only on the state.
Then derive the three edge types for edit distance. Ask them to read the edges aloud in terms of
"what did I just do to the strings".

**Slide 5: The state space is a DAG.** Walk the highlighted path (delete `f`, match `l`, `a`, `w`,
insert `n`). Point out that every edge increases $i+j$, hence acyclic, and that this is *the* property used
later (Part 4 removes it).

**Slide 6: Paths versus states (Poll).** *Ask:* "For two strings of length 20, roughly how many edit
scripts are there?" Options (a) $10^6$, (b) $10^9$, (c) $10^{12}$, (d) more than $10^{14}$.
**[reveal]** The answer is (d): $D(20,20)=260{,}543{,}813{,}797{,}441$ versus 441 states.
*Say:* "The graph is a compressed representation of the solution space: many paths share sub-paths."
Expect most people to underestimate.

**Slide 7: Dynamic programming on a DAG.** State the theorem and the proof sketch. Keep it quick for this
audience, but emphasise the two ingredients: additivity (the prefix choice does not constrain the last
edge) and acyclicity (the equations are well-founded). The last line is a promise: cyclic graphs are Part 4.

**Slide 8: A strategy for dynamic programming problems.** This is the four-step recipe: decompose, base
case plus inductive step, bottom-up evaluation, reconstruction. *Say:* "Complexity is read off the DAG:
number of states times cost per state, which is essentially the number of edges." Ask people to remember
"edges", we will come back to it in Part 3.

**Slide 9: Steps 1 to 2: the recurrence.** Present the recurrence. Explain correctness via the *last*
operation of an optimal script. Remind them that, per the contract, solutions are presented as
recurrences, never as code.

**Slide 10: Step 3: bottom-up evaluation (Predict).** Give the table with the last two rows hidden.
*Ask:* "Fill in the last two rows." Wait about a minute. **[reveal]** the filled table; highlighted cells
are the optimal path. The answer is $\mathrm{ED}=2$.

**Slide 11: Step 4: reconstruction.** Walk back from $(m,n)$ choosing the argmin. Show the aligned strings.
Mention space: rolling rows lose reconstruction, Hirschberg restores it.

**Slide 12: Complexity and its limits.** $\Theta(mn)$. Two asides for the experts: output-sensitive
algorithms (Ukkonen, Myers; Myers is behind `git diff`), and the SETH lower bound of Backurs and Indyk.
*Say:* "Conditional lower bounds tell us when to stop trying."

**Slide 13: Is the state space really sufficient? (Discuss).** *Ask:* "Allow transpositions at cost 1. Which
edge do you add? Does the complexity change? Is it exactly Damerau-Levenshtein?"
**[reveal]** Add $(i-2,j-2)\to(i,j)$; still $\Theta(mn)$; but it is *not* the true Damerau-Levenshtein distance:
it is the optimal string alignment (OSA) distance. For `CA` to `ABC`, OSA gives 3 but the true distance is 2.
*Say:* "When the state space is not sufficient, DP silently solves a different problem." The exact
algorithm is on the backup slide at the end.

**Slide 14: Part 1 in one slide.** Summarise: sufficient state, acyclicity, decomposable objective. Then
the bridge: "Now, problems where the recipe *looks* applicable."

# Part 2: Is It Dynamic Programming? Case Studies

**Slide 15: How this part works.** Explain the format: for each problem you show candidate solutions and
the room votes on correctness and on whether it is the right tool. Read the triage test: sufficient state of
polynomial size; decomposable objective; and no cheaper algorithm exploiting more structure.
*Say:* "The first two make DP correct. The third decides whether it is *good*."

## Case 1: Split Array Largest Sum (LeetCode 410), a false positive because DP is inefficient

**Slide 16: Problem.** Read the problem and the example $(7,2,5,10,8)$, $k=2$, answer 18.

**Slide 17: Candidate solutions (Vote).** Proposal A is the DP recurrence; Proposal B is the greedy that
closes a part when its sum would exceed $\lceil S_n/k\rceil$. *Ask:* "For each: correct or incorrect? If correct,
efficient?" Count hands. Many will vote B correct.

**Slide 18: Verdict.** A is correct but costs $nk$ states times $O(n)$, so $\Theta(kn^2)$. B is wrong:
$a=(1,3,2)$, $k=2$ gives 5 versus the optimum 4. *Say:* "Balancing heuristics fail because the objective is a
maximum, not a deviation from the mean."

**Slide 19: A better algorithm, and why DP is wasteful.** Decide-then-optimise: for a cap $L$, the greedy
computes the minimum number of parts $c(L)$ in $O(n)$; $c$ is monotone, so binary search on $L$ gives
$O(n\log S_n)$. Be honest: the DP is *correct*, and can be sped up to about $O(kn\log n)$ using the
monotone optimum, but it still computes $f(i,j)$ for every prefix and every part count when we want one number.
*Say:* "DP answers a question much richer than the one asked."

**Slide 20: Discussion (Discuss).** Two questions: what property of "minimise the maximum" makes the
decision version easy; what happens if we minimise the sum of squares instead. **[reveal]** The max commutes
with refinement; for sums of squares there is no threshold structure, so it is a genuine DP with a transition that
the convex hull trick can accelerate. Promise: "We will meet that transition in Part 3."

## Case 2: Maximum weight independent set on a tree (LeetCode 337), a real DP

**Slide 21: Problem.** The tree version (House Robber III); the path version is LeetCode 198. Mention that
the general problem is NP-hard, and ask "Where would you put the state?" rhetorically.

**Slide 22: Candidate solutions (Vote).** Four proposals. A: heaviest-first greedy. B: $f(v)=\max\{w_v+\sum f(c),\sum f(c)\}$.
C: grandchildren recurrence. D: two-state recurrence. *Ask:* "Which are correct? For each rejected one,
give me a counterexample." The path $2-3-2$ kills A and B.

**Slide 23: Verdict.** A and B are wrong (A returns 3 versus 4; B returns 7, which is infeasible, because $f(c)$
may itself use $c$). C and D are correct. *Say:* "D has twice the states but local transitions. Enlarging the
state to make the transition local is a standard trick."

**Slide 24: Why trees, and what generalises.** The vertex $v$ is a separator. For bounded treewidth $\omega$ the
bag state is the independent subset in the bag, giving $O(2^\omega\,\omega\,n)$ time. *Say:* "The structure of the
graph *is* the shape of the state DAG."

**Slide 25: Discussion (Discuss).** *Ask:* "Now the graph is a cycle. Fix the recurrence and give the
complexity." **[reveal]** Fix the status of vertex 1 and run the path DP twice, $\Theta(n)$; or enlarge the
state to (status of vertex 1, status of current vertex), the width-2 decomposition made explicit.

## Case 3: Minimum Initial Energy to Finish Tasks (LeetCode 1665), a false positive

**Slide 26: Problem.** Tasks $(a_i,m_i)$: need $m_i$ energy to start, finishing consumes $a_i$. The order is
free. *Say:* "It looks like a perfect DP: a sequence of decisions with additive consumption."

**Slide 27: Candidate solutions (Vote).** A: sort by $m_i$ decreasing. B: sort by $a_i$ increasing.
C: the subset recurrence $F(S)=\min_{i\in S}\max\{F(S\setminus i),A(S\setminus i)+m_i\}$.
*Ask:* "Correct? Is C a DP? Is it useful?"

**Slide 28: Verdict.** Counterexamples (tasks written $[a,m]$): $[1,2],[3,3]$ kills A (5 versus 4);
$[1,1],[2,3]$ kills B (4 versus 3). C is correct.
**Be precise here.** The subset recurrence *does* have optimal substructure, because the energy left after
a set $S$ is $E-A(S)$ whatever the order. What fails is the existence of a *compact* sufficient state:
the state has to be the whole subset, so $\Theta(2^n n)$. Frame it as "there is no polynomial-size
sufficient state", not as "there is no optimal substructure at all". If someone points this out, agree: they
are right, and it is precisely the sense in which this is a false positive.

**Slide 29: What actually solves it.** The exchange argument: sort by $\kappa_i=m_i-a_i$ decreasing. Walk the
two-line inequality for an adjacent swap, then say that bubble sort finishes the proof. $O(n\log n)$.
*Say:* "The structure that makes this easy is an exchange argument, not a state space."

**Slide 30: Rapid fire (Vote).** Six one-line problems: DP, something better, or no compact DP?
Give 20 seconds each. **[Next slide gives the answers.]**

**Slide 31: Rapid fire: answers.** (1) coin change: DP. (2) unweighted interval scheduling: greedy.
(3) longest simple path: no compact DP (NP-hard). (4) knapsack with $n=40$, huge weights: meet in the middle.
(5) LIS: DP then patience sorting. (6) shortest paths with cycles: DP in disguise, Dijkstra picks an order;
this is the theme of Part 4.

# Part 3: Going Faster

**Slide 32: Where does the time go?** Return to "time is edges". Two levers: change the recurrence,
change the compute order. *Say:* "Both are statements about the shape of the DAG."

## Lever I: change the recurrence (convex hull trick)

**Slide 33: A problem with a quadratic transition (Discuss).** Batched jobs: partition a sequence into
consecutive batches, a batch of size $s$ costs $M+s^2$. The naive recurrence is $\Theta(n^2)$.
*Ask:* "Expand the square. Which terms couple $i$ and $j$?" Wait for "only the cross term".

**Slide 34: Rewriting the recurrence.** The recurrence becomes a minimum over lines evaluated at $S_i$.
*Say:* "Everything is a function of $i$ alone or of $j$ alone, except $-2S_iS_j$." Define the line $\ell_j$.

**Slide 35: The lower envelope.** Show the plot: the dashed line $\ell_1$ never attains the minimum. Explain
that slopes decrease with insertion order, so each new line is best for large $x$.

**Slide 36: The monotone convex hull trick.** Present the deque algorithm and the redundancy test.
Explain amortisation: every line is pushed once and popped once. Why the front can be discarded: later
queries are larger, and later lines have smaller slopes. For an expert room, one sentence suffices.

**Slide 37: Robustness of the trick (Vote).** Two variants: (a) monotone slopes, arbitrary queries;
(b) arbitrary slopes and queries. **[reveal]** (a) binary search on the hull, $O(n\log n)$;
(b) Li Chao tree. Mention the 128-bit arithmetic caveat; competitive programmers will nod.

**Slide 38: What structure was exploited?** The general pattern is a separable cost. Walk the table: Monge
cost with monotone optimum, D&C optimisation, Aliens trick, Knuth-Yao. *Say:* "These are different structures
with different accelerations. The skill is recognising which structure your cost function has."

## Lever II: change the compute order (bitmask / broken profile)

**Slide 39: Tilings and bitmask DP.** Count domino tilings of an $n\times m$ board with $m$ small. Present
the row-by-row recurrence with states = masks and compatible pairs. $2^m$ states per row but $3^m$ pairs.

**Slide 40: The edge count is the bottleneck (Discuss).** *Ask:* "Only $2^m$ states, but $3^m$ edges. Can we
compute the same numbers with fewer edges? Hint: decide one cell at a time." **[reveal]** The idea of inserting
intermediate states; the profile of the next $m$ cells.

**Slide 41: Broken profile DP.** The recurrence with three cases (covered cell, vertical, horizontal). Time
$\Theta(nm2^m)$ versus $\Theta(n3^m)$.

**Slide 42: Same values, different DAG.** The picture and the table for $m=16$: states grow by a factor of 16,
edges drop by about 14 times. *Say:* "Cost is edges, not states. This is why Part 1 said to remember that."

**Slide 43: Predict (Predict).** *Ask:* "How many domino tilings does the $8\times8$ board have, and the board
with opposite corners removed?" Options $10^4$, $10^5$, $10^7$, $10^9$. **[reveal]** $12{,}988{,}816=(4\cdot901)^2$
and $0$ for the mutilated board (colouring argument). Mention honestly that for planar grids Kasteleyn's
Pfaffian method is polynomial, so DP is not the best tool for plain tilings; its advantage is that it handles
arbitrary local constraints.

**Slide 44: Part 3 in one slide.** The two-column comparison. Close with "the recurrence's meaning did not change".

# Part 4: Beyond Programming Problems: MDPs

**Slide 45: Where dynamic programming was invented.** Bellman's setting: sequential decisions under
uncertainty. Three assumptions are about to break: determinism, acyclicity, enumerable state space.

**Slide 46: Markov decision processes.** Define $(\mathcal S,\mathcal A,P,r,\gamma)$, policies, returns,
$v_\pi$ and $q_\pi$. *Say:* "The Markov property is exactly the sufficiency condition from Part 1."

**Slide 47: Edit distance as an MDP (Discuss).** *Ask:* "Cast edit distance as an MDP." **[reveal]**
Deterministic, $\gamma=1$, episodic. Then the twist: if the diagonal action can fail, $(i,j)$ gets a self-loop
and the graph is no longer a DAG.

**Slide 48: The Bellman expectation equation.** Derive it from $G_t=R_{t+1}+\gamma G_{t+1}$. Show the matrix
form $v_\pi=(I-\gamma P_\pi)^{-1}r_\pi$. *Say:* "It is a simultaneous linear system, not an ordered recurrence."

**Slide 49: The Bellman optimality equation.** The $\max$ version. Map the pieces: optimal substructure becomes
the principle of optimality; reconstruction is the $\arg\max$; deterministic transitions with $\gamma=1$ recover
the DAG recurrence of Part 1.

**Slide 50: Finite horizon: Part 1 verbatim.** Backward induction over $(t,s)$ is the recipe verbatim: base
case $V_T$, inductive step, bottom-up, argmax. The only new feature is the AND/OR structure (we choose, then
nature chooses).

**Slide 51: Infinite horizon: the DAG disappears.** Introduce the Bellman operator and the contraction
theorem. Walk the one-line proof using $|\max f-\max g|\le\max|f-g|$. *Say:* "The contraction replaces the base
case: it gives a unique fixed point and convergence from any starting point."

**Slide 52: What replaces each ingredient? (Discuss).** Ask them to fill in the right column of the table before
you reveal it: base case, evaluation order, complexity, reconstruction. **[reveal]**

**Slide 53: Value iteration.** The iteration, the a-posteriori error bound $\tfrac{\gamma}{1-\gamma}\|V_{k+1}-V_k\|_\infty$
with its two-line proof, and the greedy-policy loss bound. Plant the observation: the *policy* stabilises long
before the *values*.

**Slide 54: A worked example (Predict).** Two-state MDP, $\gamma=0.9$. *Ask:* "From $V_1$, which action does the greedy
policy choose at $s_1$?" **[reveal]** $q(s_1,a)=1.9$, $q(s_1,b)=2.34$, so $b$. Point at the table: after one sweep the
greedy policy is already optimal, while the values are still far from $17.561$.

**Slide 55: Policy iteration.** Evaluation plus improvement; the policy improvement theorem with its monotonicity
proof sketch. In the example, starting from $(a,c)$, two iterations suffice. Mention Ye's strongly polynomial bound.

**Slide 56: Asynchronous dynamic programming.** The three assumptions of synchronous DP: global sweeps,
a fixed schedule with barriers, all states equally worth updating. Define the asynchronous update.

**Slide 57: Schedules for asynchronous updates.** Gauss-Seidel, prioritised sweeping, RTDP, heuristic search.
*Say:* "Dijkstra is asynchronous value iteration with a particular schedule; Bellman-Ford is the synchronous one."
This answers question 3 of slide 2.

**Slide 58: Choose your schedule (Predict).** The four-node chain with a shortcut edge. *Ask:* "How many
synchronous sweeps until $V(a)$ is final? Which update order finishes in one pass?" **[reveal]** Three sweeps;
the order $c,b,a$. *Say:* "Bottom-up evaluation from Part 1 is the best asynchronous schedule when the graph is a DAG."

**Slide 59: Why asynchronous updates converge.** State the theorem (Bertsekas; Bertsekas-Tsitsiklis) and walk
through the nested-box argument. This is the most advanced slide of the day: slow down, and do not skip the
induction.

**Slide 60: From asynchronous DP to learning.** Replace the expectation with a sample to get Q-learning, the
stochastic-approximation version of asynchronous value iteration. Walk the three rows: what carries over, what
changes, what breaks (function approximation).

**Slide 61: Discussion (Discuss).** An open question: with $10^{20}$ states, which requirement from Part 1 do you
give up, what replaces the table, and which guarantee of the contraction proof do you lose? Let the discussion
run; this is a good place to take a short break afterwards.

# Part 5: Other Places, and What We Learned

**Slide 62: One DAG, many semirings.** The DAG recurrence over a semiring $(K,\oplus,\otimes)$. Connect the earlier
examples: edit distance is $(\min,+)$, tilings $(+,\times)$, split array $(\min,\max)$.

**Slide 63: Name the semiring (Vote).** Three recurrences: path counting, split array, and finite-horizon
backward induction for MDPs. **[reveal]** $(+,\times)$; $(\min,\max)$; and the MDP one is *not* a semiring
path problem because it alternates $\max$ with an expectation (an AND/OR graph). For a fixed policy it becomes
linear.

**Slide 64: Linear optimisation: a staircase LP.** Production planning LP. The constraints couple consecutive
periods only, so the inventory is a sufficient state and the LP admits a DP with a *real-valued* state.

**Slide 65: The value functions are piecewise linear and convex.** Show the plot of $V_1$ and $V_2$ and the
theorem. *Say:* "DP is exact here without discretisation: represent $V_t$ by its breakpoints or its supporting lines."

**Slide 66: Reading the slopes (Predict).** *Ask:* "Explain the slopes $-3$, $0$, $+2$ of $V_2$." **[reveal]** An extra
unit of inventory saves a unit of production at cost 3; then it replaces production at 1 but costs holding 1 (net 0);
then it is just held for two periods (cost 2). Slopes are the dual variables, which is how SDDP builds cuts.

**Slide 67: The other direction: MDPs as linear programs.** The LP whose optimum is $v^\star$ (monotonicity
argument), and its dual in terms of occupancy measures. *Say:* "Bellman optimality *is* the LP optimality
condition, via complementary slackness."

**Slide 68: Dynamic programming elsewhere.** Skim the table: bioinformatics, NLP, probabilistic inference, control,
operations research, economics. One sentence each.

**Slide 69: Your turn (Pair).** Three minutes with a neighbour about a problem from their own work: state, DAG or
cycles, semiring, edge count, which lever. Collect two or three, *including ones that failed the test*.

**Slide 70: What we learned.** Read the five takeaways, linking them to slides 8, 30, 44, 51 to 59, and 62 to 67.

**Slide 71: Questions and discussion.** Open floor.

# Appendix (unnumbered)

- **References (1/2), (2/2):** sources for the results quoted in the talk.
- **Backup: the Damerau-Levenshtein state:** the exact (unrestricted) algorithm with last-occurrence tables.
  Use it if someone asks about slide 13.

# Facts worth double-checking before you present

- The remarks on LeetCode problems (numbers 410, 337, 198, 1665) are from memory of the problem statements; check
  the current statements and constraints if you plan to quote them.
- All recurrences in the deck were checked against brute force with a small script before the deck was built
  (edit distance, OSA versus true Damerau-Levenshtein, split array, tree independent sets, subset recurrence and
  exchange order for the energy problem, convex hull trick, domino tilings, the MDP example, and the production LP).
