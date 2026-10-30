# Beyond Dynamic Programming: Presenter's Guide

This guide accompanies `presentation.tex` / `presentation.pdf`. The slides carry only what the
audience needs to see: definitions, recurrences, tables, and questions. Everything you say
(motivation, intuition, asides, bridges between slides, how to run each question) is in this guide.

- **Slide numbers below are the numbers printed in the slide footer.** The title slide and the five
  section dividers are unnumbered. The Outline is slide 1. The closing "Questions and discussion"
  slide (118) is counted but shows no number.
- There are no overlays: every slide is a single PDF page. Each question sits on its own slide, and
  the answer is on the following slide(s).
- Total: 118 numbered slides + 6 unnumbered ones, plus a 3-slide appendix (references, backup).
- The previous, denser version of the deck is kept as `presentation_audience.pdf`. Its slide
  numbers differ from the ones below.
- Suggested time: about 2.5 to 3 hours with discussion. Rough budget: Part 1 35 min, Part 2 40 min,
  Part 3 35 min, Part 4 45 min, Part 5 15 min. There are more slides than before but the same
  material, so the timing is unchanged; many slides now take under a minute.

## How the interaction works

A question slide shows a single teal box. The box title names the topic of the question
(for example *How many edit scripts?* or *Choose the data structure*). The slide does not say how
the room should answer; you announce that. Each question slide in the script below is tagged with
one of five formats:

| Format | What the audience does | How to run it |
|--------|------------------------|---------------|
| **Poll** | Show of hands on a multiple-choice question | Read the options, count hands, then move to the answer slide |
| **Vote** | Decide correct / incorrect (or choose between options) for each candidate | Ask for hands for each candidate in turn; make one person who voted *incorrect* produce a counterexample before you advance |
| **Predict** | Commit to a prediction before seeing the answer | Give 30 to 60 seconds of silence, take two or three guesses, then advance |
| **Discuss** | Open question; the worked answer is on the next slide | Take answers from the room first; only advance when the discussion stalls |
| **Pair** | Talk to a neighbour for a few minutes | Use a timer (3 min), then collect two or three reports |

Question slides: 2, 7, 14, 19, 27, 32, 36, 40, 44, 48, 52, 58, 64, 69, 75, 84, 89, 97, 103, 106,
111 and 116. The longest stretches without a question are slides 89 to 97 and 75 to 84 in Part 4;
most of those slides are short, so the room is never more than about ten minutes from a question.
If you want an extra interaction there, ask the room to predict the next line of a proof.

**General tips**

- Do not advance early. In the case studies the whole point is that people disagree.
- Wrong answers are valuable here: the audience is expert, so many people will confidently vote for a
  plausible but incorrect proposal. Ask *why* before you advance to the counterexample.
- If the room is quiet, call on the two groups explicitly: "the competitive programmers" and
  "the people from research". They often disagree about what counts as an efficient solution.
- Candidate-solution slides (26, 35, 43) are followed by a separate verdict question slide. Leave the
  candidates up long enough for people to read them, then advance to the question and run the vote.
  Go back one slide if people ask to see the candidates again.

## Building the slides

```bash
lualatex presentation.tex   # run twice so the outline and numbering settle
```

The deck uses `lualatex` and the Metropolis theme, as the template README describes. Replace `Your Name`
and `Your Affiliation` in `presentation.tex`. The placeholder university logo from the template is
commented out (`\titlegraphic`). The question box is `\audience[<title>]{<question>}` in
`setup-workshop.tex`; the title is optional.

---

# Part 0: Opening

**Title slide.** Welcome the room. State the contract: this is a workshop, not a lecture, and the
goal is that *everyone* leaves with at least one idea about dynamic programming they did not have before.

**Slide 1: Outline.** Read the five parts. Say that Part 1 is brief and is there to fix notation
(the state-space view), and that the most unusual material is in Parts 3 and 4.

**Slide 2: Calibration (Poll).** *Say first:* "This workshop assumes strong programming and discrete
mathematics. All recurrences are written mathematically, never as code." Then ask for a show of hands on
each of the three questions: convex hull trick / Li Chao tree; Bellman optimality from memory;
"is Dijkstra a DP?".
*Say:* "This tells me where to spend time. If many hands go up for the first, I will go quicker on slide 56.
If few go up for the second, I will slow down in Part 4." Note the hands for question 3: the answer
(yes, it is value iteration with a clever schedule) comes in slides 49 and 96.

# Part 1: Dynamic Programming from First Principles

**Slide 3: A motivating problem: minimum edit script.** *Say:* "Think version control, `git diff`. We
have two versions of a file as token sequences and want the cheapest sequence of insertions, deletions
and substitutions." Then the key point, which is no longer on the slide: a script is a sequence of at
most $m+n$ local decisions, so the number of scripts is exponential. *Ask rhetorically:* "What is a good
way to organise the search?" Do not mention the recurrence yet.

**Slide 4: State spaces.** The definition. *Say:* "A state space is a graph whose paths are solutions.
A state is a summary of a partial solution." Dwell on **sufficiency**: the future must depend only on the
state, not on how we got there.

**Slide 5: The edit-distance state space.** Derive the three edge types. Ask the room to read the edges
aloud in terms of "what did I just do to the strings".

**Slide 6: The state space is a DAG.** Walk the highlighted path: delete `f`, match `l`, `a`, `w`, insert `n`,
total cost 2. Point out that every edge increases $i+j$, hence acyclic; a topological order is row-major,
or any order by $i+j$. Stress that acyclicity is *the* property used later (Part 4 removes it).

**Slide 7: Paths versus states (Poll).** *Ask:* "For two strings of length 20, roughly how many edit
scripts are there?" Count hands for (a) to (d). Expect most people to underestimate.

**Slide 8: Paths versus states: answer.** The answer is (d): $D(20,20)\approx 2.6\cdot10^{14}$ paths versus
441 states. *Say:* "Dynamic programming exists because many paths share the same sub-paths. The state graph
is a compressed representation of the solution space."

**Slide 9: Dynamic programming on a DAG.** State the theorem. Keep it quick for this audience.

**Slide 10: Why it works.** The two ingredients: additivity (the prefix choice does not constrain the last
edge, so minimising each prefix is lossless) and acyclicity (the equations are well-founded, induct along a
topological order). *Say, as a promise:* "Cyclic graphs satisfy the same equations, but the equations no
longer *define* $d$ by induction. That is Part 4."

**Slide 11: A strategy for dynamic programming problems.** The four-step recipe. For step 4, add that
you can either record the minimising predecessor of each state or recompute it on the way back.

**Slide 12: Complexity is read off the DAG.** *Say:* "Number of states times cost per state, which is
essentially the number of edges." Ask people to remember "edges"; it returns in Part 3.

**Slide 13: Steps 1 to 2: the recurrence.** Present the recurrence. Explain correctness out loud via the
*last* operation: consider the last operation of an optimal script for $(x_{1..i},y_{1..j})$. It is one of
three kinds, and the script before it is a script for the corresponding smaller pair. That script must
itself be optimal, otherwise it could be replaced by a cheaper one.

**Slide 14: Step 3: bottom-up evaluation (Predict).** *Say:* "We evaluate row by row, which is a
topological order. Rows are `flaw`, columns are `lawn`." *Ask:* "Fill in the last two rows." Wait about a
minute.

**Slide 15: Step 3: filled in.** The filled table. The highlighted cells are on the optimal path. $\mathrm{ED}=2$.

**Slide 16: Step 4: reconstruction.** Walk back from $(m,n)$ choosing the argmin, and read the alignment.

**Slide 17: Memory.** Recomputing the argmin needs only the value table. Rolling rows lose reconstruction;
Hirschberg restores it.

**Slide 18: Complexity and its limits.** $\Theta(mn)$. Two asides for the experts: output-sensitive
algorithms (Ukkonen, Myers; Myers is behind `git diff`), and the SETH lower bound of Backurs and Indyk.
*Say the lesson:* "The state space fixes a baseline. Structure of the instance or of the cost function can
beat it, and conditional lower bounds tell us when to stop trying."

**Slide 19: Is the state space really sufficient? (Discuss).** Transpositions at cost 1. Three
questions: which edge, does the complexity change, is it exactly Damerau-Levenshtein?

**Slide 20: Transpositions: the edge.** Add $(i-2,j-2)\to(i,j)$; still $\Theta(mn)$.

**Slide 21: Transpositions: not Damerau-Levenshtein.** It is the optimal string alignment (OSA) distance.
For `CA` to `ABC`, OSA gives 3 but the true distance is 2.
*Say:* "When the state space is not sufficient, DP silently solves a different problem." The exact
algorithm is on the backup slide at the end.

**Slide 22: Part 1 in one slide.** Summarise: sufficient state, acyclicity, decomposable objective.
*Say:* "Design is in step 1. Steps 2 to 4 are mechanical once the state space is right, and so is the
complexity." Then the bridge: "Next, problems where the recipe *looks* applicable. Does it help?"

# Part 2: Is It Dynamic Programming? Case Studies

**Slide 23: How this part works.** Explain the format: for each problem you show candidate solutions and
the room votes on correctness and on whether it is the right tool.

**Slide 24: Triage test for dynamic programming.** Read the three questions.
*Say:* "The first two make DP correct. The third decides whether it is *good*."

## Case 1: Split Array Largest Sum (LeetCode 410), a false positive because DP is inefficient

**Slide 25: Problem.** Read the problem and the example $(7,2,5,10,8)$, $k=2$, answer 18. *Say:* "We will
look at two proposals."

**Slide 26: Candidate solutions.** Proposal A is the DP recurrence; Proposal B is the greedy that
closes a part when its sum would exceed $\lceil S_n/k\rceil$. Give people time to read both.

**Slide 27: Your verdict (Vote).** *Ask:* "For each: correct or incorrect? If correct, efficient?" Count
hands. Many will vote B correct.

**Slide 28: Proposal A is correct, but slow.** $nk$ states times $O(n)$, so $\Theta(kn^2)$.

**Slide 29: Proposal B is wrong.** $a=(1,3,2)$, $k=2$ gives 5 versus the optimum 4. *Say:* "Greedy
heuristics that balance parts fail because the objective is a maximum, not a deviation from the mean."
*Ask rhetorically:* "Is there a correct greedy at all?"

**Slide 30: Decide, then optimise.** For a cap $L$, the greedy computes the minimum number of parts
$c(L)$ in $O(n)$. Justify it aloud: it stays ahead of any other partition. $c$ is monotone, so binary search
on $L$ gives the answer.

**Slide 31: Comparison.** Be honest: the DP is *correct*, and can be sped up to about $O(kn\log n)$ using the
monotone optimum. But it computes $f(i,j)$ for every prefix and every part count when we need one number.
*Say:* "DP answers a question much richer than the one asked."

**Slide 32: Why the maximum helps (Discuss).** Two questions: what property of "minimise the maximum"
makes the decision version easy; what happens if we minimise the sum of squares instead.

**Slide 33: Answers.** The max commutes with refinement; for sums of squares there is no threshold
structure, so it is a genuine DP with a transition that the convex hull trick can accelerate.
*Say:* "We will meet that transition in Part 3."

## Case 2: Maximum weight independent set on a tree (LeetCode 337), a real DP

**Slide 34: Problem.** The tree version (House Robber III); the path version is LeetCode 198. *Say:* "For
general graphs this problem is NP-hard. On a tree, the subtree of $v$ is separated from the rest of the tree
by the single vertex $v$." *Ask rhetorically:* "Where would you put the state?"

**Slide 35: Candidate solutions.** Four proposals. A: heaviest-first greedy. B: $f(v)=\max\{w_v+\sum f(c),\sum f(c)\}$.
C: grandchildren recurrence. D: two-state recurrence. Give people time to read them.

**Slide 36: Your verdict (Vote).** *Ask:* "Which are correct? For each rejected one, give me a
counterexample." The path $2-3-2$ kills A and B.

**Slide 37: A and B are wrong.** A returns 3 versus 4; B returns 7, which is infeasible, because $f(c)$
may itself use $c$.

**Slide 38: C and D are correct.** *Say:* "Both have the same state DAG, the rooted tree. D has twice the
states but local transitions. Enlarging the state to make the transition local is a standard trick."

**Slide 39: What generalises.** *Say:* "The tree makes $v$ a separator: the two sides interact only
through $v$'s status." For bounded treewidth $\omega$ the bag state is the independent subset in the bag.
*Say:* "The structure of the graph *is* the shape of the state DAG. Same recipe, different shape."

**Slide 40: From trees to cycles (Discuss).** *Ask:* "Now the graph is a cycle. Fix the recurrence and give
the complexity."

**Slide 41: The cycle.** Fix the status of vertex 1 and run the path DP twice, $\Theta(n)$; or enlarge the
state to (status of vertex 1, status of current vertex). *Say:* "That second version is the width-2 tree
decomposition made explicit."

## Case 3: Minimum Initial Energy to Finish Tasks (LeetCode 1665), a false positive

**Slide 42: Problem.** Tasks $(a_i,m_i)$: need $m_i$ energy to start, finishing consumes $a_i$. *Say:* "The
order is a free choice, and there are $n!$ orders. It looks like a perfect candidate for DP: a sequence of
decisions with additive consumption."

**Slide 43: Candidate solutions.** A: sort by $m_i$ decreasing. B: sort by $a_i$ increasing.
C: the subset recurrence.

**Slide 44: Your verdict (Vote).** *Ask:* "Correct? Is C a DP? Is it useful?"

**Slide 45: A and B are wrong.** $[1,2],[3,3]$ kills A (5 versus 4); $[1,1],[2,3]$ kills B (4 versus 3).

**Slide 46: C is correct, but does not compress.** **Be precise here.** The subset recurrence *does* have
optimal substructure, because the energy left after a set $S$ is $E-A(S)$ whatever the order. What fails is
the existence of a *compact* sufficient state: the state has to be the whole subset, so $\Theta(2^n n)$.
*Say:* "The formulation is valid, but the state does not compress. That is the sense in which this is a
false positive." If someone points out that the DP has optimal substructure, agree: they are right.

**Slide 47: An exchange argument.** Sort by $\kappa_i=m_i-a_i$ decreasing. Walk the two-line inequality
for an adjacent swap (energy before and after the pair is the same in both orders), then say that repeated
swaps sort the sequence, as in bubble sort, without increasing the requirement. $O(n\log n)$.
*Say:* "The structure that makes this easy is an exchange argument, not a state space."

**Slide 48: Rapid fire (Vote).** Six one-line problems: DP, something better, or no compact DP?
Give 20 seconds each.

**Slide 49: Rapid fire: answers.** Add the reasons aloud:
(1) coin change: DP, state is the remaining amount.
(2) unweighted interval scheduling: earliest-finish-time greedy; DP is valid but unnecessary.
(3) longest simple path: NP-hard; the state has to record the visited set.
(4) knapsack with $n=40$ and huge weights: $O(nW)$ is hopeless, meet in the middle works.
(5) LIS: $O(n^2)$ DP, then the changed state gives $O(n\log n)$ by patience sorting.
(6) shortest paths with cycles: the Bellman equations hold but are cyclic; Dijkstra picks an evaluation
order. This is the theme of Part 4, and the answer to calibration question 3.

# Part 3: Going Faster

**Slide 50: Where does the time go?** Return to "time is edges". Two levers, about the shape of the DAG,
not its meaning: lever I uses algebra on the transition to turn a minimum over all predecessors into a
data-structure query; lever II refines the DAG with intermediate states that compute the same values.
*Say:* "We illustrate I with the convex hull trick and II with a bitmask profile DP."

## Lever I: change the recurrence (convex hull trick)

**Slide 51: A problem with a quadratic transition.** Batched jobs: partition a sequence into consecutive
batches; a batch of size $s$ costs $M+s^2$, a set-up cost plus a quadratic penalty. *Say:* "For
$n=10^6$, $\Theta(n^2)$ is hopeless."

**Slide 52: Where is the coupling? (Discuss).** *Ask:* "Expand the square. Which terms couple $i$ and
$j$?" Wait for "only the cross term".

**Slide 53: Rewriting the recurrence.** *Say:* "Everything else is a function of $i$ alone or of $j$
alone."

**Slide 54: A minimum over lines.** Define $\ell_j$. Point out that line $\ell_i$ only becomes available
after $\mathrm{dp}(i)$ is known: an online insertion.

**Slide 55: The lower envelope.** Explain the plot: slopes decrease with insertion order ($m_j=-2S_j$, and
$S_j$ is non-decreasing), so each new line is the best one for large $x$. The dashed line $\ell_1$ never
attains the minimum and may be discarded.

**Slide 56: The monotone convex hull trick.** Present the deque algorithm. Explain amortisation: every line
is pushed once and popped at most once. Why the front can be discarded: later queries are larger and later
lines have smaller slopes. For an expert room, one sentence suffices.

**Slide 57: The redundancy test.** The cross-multiplied intersection test. *Say:* "This avoids division."

**Slide 58: When the data are not monotone (Vote).** Two variants: (a) monotone slopes, arbitrary queries;
(b) arbitrary slopes and queries. Ask for a show of hands per data structure suggestion.

**Slide 59: Answers.** (a) binary search on the hull, $O(n\log n)$; (b) Li Chao tree. Add aloud: compress
coordinates offline if the query domain is large. Implementation caveat: the redundancy test multiplies
differences of intercepts and slopes; for $S\sim10^9$ the products need 128-bit arithmetic. Competitive
programmers will nod.

**Slide 60: What structure was exploited?** The general separable form.

**Slide 61: A catalogue of speed-ups.** Walk the table: Monge cost with monotone optimum, D&C optimisation,
Aliens trick, Knuth-Yao. *Say:* "These are different structures with different accelerations. The skill is
recognising which structure your cost function has."

## Lever II: change the compute order (bitmask / broken profile)

**Slide 62: Domino tilings.** Motivate it as floorplanning. Count domino tilings of an $n\times m$ board
with $m$ small. Explain the row-by-row state.

**Slide 63: Row-by-row recurrence.** Compatible pairs: the cells outside $M\cup M'$ must split into runs of
even length because they are tiled by horizontal dominoes. $2^m$ states per row but $3^m$ pairs.

**Slide 64: The edge count is the bottleneck (Discuss).** *Ask:* "Only $2^m$ states, but $3^m$ edges. Can we
compute the same numbers with fewer edges?" If the room stalls, give the hint: "What if we decide one cell
at a time?"

**Slide 65: One cell at a time.** Insert intermediate states between rows; the state is the profile of the
next $m$ cells.

**Slide 66: Broken profile DP.** The state and the base case.

**Slide 67: Broken profile DP: transitions.** Three cases: covered cell, vertical, horizontal. Blocked cells:
forbid dominoes that would cover them and let a blocked cell pass through unchanged.
$\Theta(nm2^m)$ versus $\Theta(n3^m)$.

**Slide 68: Same values, different DAG.** The picture and the table for $m=16$. *Say:* "The state count grows
by a factor of $m=16$ while the edge count drops by about 14 times. Cost is edges, not states. This is why
Part 1 said to remember that."

**Slide 69: Counting tilings (Predict).** *Ask:* "How many domino tilings does the $8\times8$ board have, and
the board with opposite corners removed?" Take guesses for (a) to (d).

**Slide 70: Answer.** $12{,}988{,}816=(4\cdot901)^2$, and $0$ for the mutilated board. *Say:* "The DP is an
exact certificate of the colouring argument." Mention honestly that for planar grids Kasteleyn's Pfaffian
method counts tilings in polynomial time, so DP is not the best tool for plain tilings; DP is exponential in
$m$ but handles non-planar constraints and arbitrary local rules.

**Slide 71: Part 3 in one slide.** The two-column comparison. *Say:* "Both are statements about the DAG.
In both cases the recurrence's meaning did not change."

# Part 4: Beyond Programming Problems: MDPs

**Slide 72: Where dynamic programming was invented.** Bellman's setting: an agent observes a state, acts,
receives a reward, and the environment moves stochastically. Three assumptions break: edges become
distributions; unbounded horizons create cycles; state spaces become too large to sweep (Go, robotics,
datacentre control). *Say:* "We keep the ideas, state, optimal substructure, recurrence, reconstruction,
and see what each becomes."

**Slide 73: Markov decision processes.** The definition. *Say:* "The Markov property is exactly the
sufficiency condition from Part 1: the future depends on the past only through $S_t$."

**Slide 74: Policies and values.** Policies, returns, $v_\pi$ and $q_\pi$, and the goal $v^\star$.

**Slide 75: Edit distance as an MDP (Discuss).** *Ask:* "Cast edit distance as an MDP." Then the twist:
the diagonal action can fail.

**Slide 76: Answer.** Deterministic, $\gamma=1$, episodic. With failures, $(i,j)$ gets a self-loop and the
graph is no longer a DAG.

**Slide 77: The Bellman expectation equation.** Derive it from $G_t=R_{t+1}+\gamma G_{t+1}$.

**Slide 78: Matrix form.** $v_\pi=(I-\gamma P_\pi)^{-1}r_\pi$; equivalently $v_\pi=\sum_k\gamma^kP_\pi^kr_\pi$.
*Say:* "This is the analogue of evaluating every state from its predecessors, but simultaneous rather than
ordered."

**Slide 79: The Bellman optimality equation.** The $\max$ version, and the greedy policy.

**Slide 80: Back to Part 1.** Map the pieces: optimal substructure becomes the principle of optimality;
reconstruction is the $\arg\max$; deterministic transitions with $\gamma=1$ recover the DAG recurrence of
Part 1 (with $\max$ and $\min$ swapped).

**Slide 81: Finite horizon: Part 1 verbatim.** Backward induction over $(t,s)$ is the recipe verbatim. *Say
the one new feature:* "The dependency structure is an AND/OR graph. A max node, where we choose, feeds an
expectation node, where nature chooses."

**Slide 82: Infinite horizon: the DAG disappears.** Introduce the Bellman operator.

**Slide 83: The Bellman operator is a contraction.** Walk the one-line proof using
$|\max f-\max g|\le\max|f-g|$. *Say:* "The contraction replaces the base case: it gives a unique fixed
point and convergence from any starting point."

**Slide 84: What replaces each ingredient? (Discuss).** Ask the room to answer for each of the four
recipe elements before you advance.

**Slide 85: Answer.** The completed table. Compare it with what the room said.

**Slide 86: Value iteration.** The iteration and the cost of one sweep.

**Slide 87: When to stop.** The a-posteriori error bound with its two-line proof, and the greedy-policy loss
bound. Plant the observation: "The *policy* stabilises long before the *values* do."

**Slide 88: A worked example.** Two-state MDP, $\gamma=0.9$. Read the actions.

**Slide 89: The greedy action (Predict).** *Ask:* "From $V_1$, which action does the greedy policy choose at
$s_1$?"

**Slide 90: Answer.** $q(s_1,a)=1.9$, $q(s_1,b)=2.34$, so $b$. Point at the table: after one sweep the greedy
policy is already optimal, while the values are still far from $17.561$. This is the observation from slide 87.

**Slide 91: Policy iteration.** Evaluation plus improvement; the policy improvement theorem with its
monotonicity proof sketch. *Say:* "There are finitely many policies and each step strictly improves, so it
terminates; in practice only a handful of iterations. Ye (2011) showed it is strongly polynomial for fixed
$\gamma$."

**Slide 92: Policy iteration on the example.** Two iterations suffice.

**Slide 93: Asynchronous dynamic programming.** *Say:* "Value iteration is synchronous: $V_{k+1}$ is
computed entirely from $V_k$, Jacobi-style, and every sweep visits every state." The fixed schedule with a
barrier amounts to a global clock in distributed computation.

**Slide 94: The asynchronous update.** Define it. Order, frequency and even staleness are free.

**Slide 95: Schedules for asynchronous updates.** Add details aloud: Gauss-Seidel typically needs fewer
sweeps; prioritised sweeping enqueues the predecessors of a state when its value changes; RTDP with an
optimistic initialisation converges on the states reachable under optimal policies and never touches the rest.

**Slide 96: Dijkstra is asynchronous value iteration.** This answers question 3 of slide 2.

**Slide 97: Choose your schedule (Predict).** The four-node chain with a shortcut edge. *Ask:* "How many
synchronous sweeps until $V(a)$ is final? Which update order finishes in one pass?"

**Slide 98: Answer.** Three sweeps; the order $c,b,a$. *Say:* "For cyclic graphs, Dijkstra's priority queue
computes such an order on the fly."

**Slide 99: Why asynchronous updates converge.** State the theorem (Bertsekas; Bertsekas-Tsitsiklis).

**Slide 100: The box argument.** Walk through the nested-box argument. This is the most advanced slide of
the day: slow down, and do not skip the induction. *Close with:* "The per-coordinate contraction is all we
need: no global clock, order, or synchronisation."

**Slide 101: From asynchronous DP to learning.** Replace the expectation with a sample to get Q-learning.
*Say:* "This is asynchronous value iteration on $q^\star$ with stochastic approximation."

**Slide 102: What carries over to learning.** Walk the three rows: what carries over, what changes, what
breaks (function approximation).

**Slide 103: $10^{20}$ states (Discuss).** An open question: which requirement from Part 1 do you give up,
what replaces the table, and which guarantee of the contraction proof do you lose? Let the discussion run;
this is a good place to take a short break afterwards.

# Part 5: Other Places, and What We Learned

**Slide 104: One DAG, many semirings.** The DAG recurrence over a semiring $(K,\oplus,\otimes)$.

**Slide 105: Semirings and what they compute.** Connect the earlier examples: edit distance is $(\min,+)$,
tilings $(+,\times)$, split array $(\min,\max)$.

**Slide 106: Name the semiring (Vote).** Three recurrences: path counting, split array, and finite-horizon
backward induction for MDPs.

**Slide 107: Answers.** $(+,\times)$; $(\min,\max)$; and the MDP one is *not* a semiring path problem
because it alternates $\max$ with an expectation. *Say:* "It is a path problem on a hypergraph."

**Slide 108: Linear optimisation: a staircase LP.** Production planning, an LP in $2T$ variables. The
constraints couple consecutive periods only.

**Slide 109: The staircase LP as a DP.** Inventory is a sufficient state, so the LP is a path in a
continuous state space. *Ask rhetorically:* "There is no table. What does $V_t$ look like?"

**Slide 110: Piecewise linear and convex.** Show the plot of $V_1$ and $V_2$ and the theorem. *Proof, aloud:*
$V_t(s)$ is the optimal value of an LP whose right-hand side depends affinely on $s$; partial minimisation of
a jointly convex polyhedral function over a polyhedron is convex and polyhedral; induct.
*Say:* "DP is exact here without discretisation: represent $V_t$ by its breakpoints or its supporting lines,
which are called cuts."

**Slide 111: Reading the slopes (Predict).** *Ask:* "Explain the slopes $-3$, $0$, $+2$ of $V_2$."

**Slide 112: Answer.** Add aloud on the middle range: demand of period 2 is already covered, so the extra
unit replaces production in period 3. *Say:* "Stochastic dual dynamic programming (SDDP) builds $V_t$ from
exactly these supporting lines."

**Slide 113: MDPs as linear programs.** The LP whose optimum is $v^\star$ (monotonicity argument).

**Slide 114: The dual: occupancy measures.** Complementary slackness says $x(s,a)>0$ only if $a$ attains the
maximum in the Bellman optimality equation. *Say:* "Bellman optimality *is* the LP optimality condition."

**Slide 115: Dynamic programming elsewhere.** Skim the table, one sentence each. Extra detail if asked:
junction trees are DP over a tree decomposition of the model; OR also has DP-based algorithms for integer
programs with small coefficients; economics uses Bellman equations over continuous states.

**Slide 116: Your turn (Pair).** *Say:* "Turn to your neighbour for three minutes. Pick a problem you work
on and answer these three questions." Use a timer. Then collect two or three examples, *including ones
that failed the test*.

**Slide 117: What we learned.** Read the five takeaways, linking them to slides 11 to 12, 24 to 49, 50 to 71,
82 to 100, and 104 to 115. Expand takeaway 2 with the two counter-structures from Part 2: monotone decision
problems and exchange arguments. Expand takeaway 3: convex hull trick and broken profile. Expand takeaway 4:
Bellman operators are contractions.

**Slide 118: Questions and discussion.** Open floor.

# Appendix (unnumbered)

- **References (1/2), (2/2):** sources for the results quoted in the talk.
- **Backup: the Damerau-Levenshtein state:** the exact (unrestricted) algorithm with last-occurrence tables.
  Use it if someone asks about slides 19 to 21.

# Facts worth double-checking before you present

- The remarks on LeetCode problems (numbers 410, 337, 198, 1665) are from memory of the problem statements; check
  the current statements and constraints if you plan to quote them.
- All recurrences in the deck were checked against brute force with a small script before the deck was built
  (edit distance, OSA versus true Damerau-Levenshtein, split array, tree independent sets, subset recurrence and
  exchange order for the energy problem, convex hull trick, domino tilings, the MDP example, and the production LP).
