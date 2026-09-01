# GATE CS/IT — Knowledge Base
**Owner status:** Never attempted GATE before. 3rd year of college now → **Attempt 1 = GATE 2027** (first-ever attempt, treat as calibration) → 4th year → **Attempt 2 = GATE 2028** (serious attempt, built on Attempt 1's data)
**Target:** 70–80 percentile (realistically translates to ~55–65+ marks / 100, depending on that year's difficulty and normalization)
**Last verified:** Aug 2026 — re-check the official brochure at the start of each admission cycle since syllabus tweaks and new optional papers get added most years.

---

## 1. Exam Pattern (verify against the official GATE brochure each year — conducting IIT rotates)

| Item | Detail |
|---|---|
| Mode | Computer-Based Test (CBT) |
| Duration | 3 hours (180 min) |
| Total marks | 100 |
| Total questions | 65 |
| Question types | MCQ (single correct), MSQ (multi-select, no negative marking), NAT (numerical answer type, no negative marking) |
| Section split | General Aptitude: 15 marks · Engineering Mathematics: ~13 marks · Core CS/IT: ~72 marks |
| Negative marking | MCQ 1-mark Q: **-1/3** for wrong · MCQ 2-mark Q: **-2/3** for wrong · MSQ/NAT: **no negative marking** |
| Partial marking | None — MCQ/NAT are all-or-nothing; MSQ generally all-or-nothing unless explicitly stated otherwise for a given year |
| Calculator | Only the on-screen virtual calculator provided in the CBT interface |

**Implication for strategy:** guessing on MCQs has negative expected value unless you can eliminate ≥2 of 4 options. MSQ/NAT are safe to attempt with partial confidence — no downside. Your agent should track accuracy separately by question type.

---

## 2. Syllabus — 10 Sections

### A. General Aptitude (common to all papers, ~15 marks)
- Verbal: English grammar, sentence completion, verbal analogies, word groups, critical reasoning, reading comprehension
- Quantitative: data interpretation, ratio/proportion, percentages, permutation-combination, probability, mensuration, series
- Analytical: logical reasoning, puzzles, spatial reasoning

### B. Engineering Mathematics (~13 marks)
- Discrete Mathematics: propositional/first-order logic, sets/relations/functions, partial orders/lattices, monoids/groups, graph theory (connectivity, matching, coloring), combinatorics (counting, recurrence relations, generating functions)
- Linear Algebra: matrices, determinants, system of linear equations, eigenvalues/eigenvectors, LU decomposition
- Calculus: limits/continuity/differentiability, maxima-minima, integration, mean value theorems
- Probability & Statistics: random variables, distributions (uniform/normal/binomial/Poisson/exponential), mean/median/mode/variance, conditional probability, Bayes theorem

### C. Digital Logic
- Boolean algebra, minimization (K-map)
- Combinational circuits: multiplexers, decoders, adders
- Sequential circuits: latches, flip-flops, counters, registers
- Number representation and computer arithmetic (fixed/floating point)

### D. Computer Organization & Architecture
- Machine instructions and addressing modes
- ALU, data-path, control unit
- Instruction pipelining, pipeline hazards
- Memory hierarchy: cache (mapping, mapping, replacement), main memory, secondary storage
- I/O interfacing (DMA, interrupts)

### E. Programming & Data Structures
- Programming in C: recursion, pointers, functions, scope
- Data structures: arrays, stacks, queues, linked lists, trees (BST, AVL, B/B+ trees), heaps, graphs, hashing

### F. Algorithms
- Asymptotic complexity, recurrences (Master theorem)
- Searching, sorting, hashing
- Greedy, dynamic programming, divide-and-conquer
- Graph algorithms: BFS/DFS, shortest paths (Dijkstra, Bellman-Ford), MST (Kruskal, Prim)

### G. Theory of Computation
- Regular languages, DFA/NFA, regular expressions, pumping lemma
- Context-free languages, PDA, CNF
- Turing machines, decidability, recursively enumerable/recursive languages, undecidability (halting problem)

### H. Compiler Design
- Lexical analysis, parsing (LL, LR), syntax-directed translation
- Intermediate code generation, runtime environments
- Code optimization basics

### I. Operating Systems
- Processes, threads, scheduling (FCFS, SJF, RR, priority, MLFQ)
- Synchronization: semaphores, monitors, classical problems (producer-consumer, dining philosophers, readers-writers)
- Deadlock: detection, prevention, avoidance (Banker's algorithm)
- Memory management: paging, segmentation, virtual memory, page replacement (FIFO, LRU, Optimal)
- File systems basics

### J. Databases
- ER model, relational model, relational algebra, SQL
- Normalization (1NF–BCNF), functional dependencies
- Transactions, ACID, concurrency control, indexing (B-trees), file organization

### K. Computer Networks
- OSI/TCP-IP layers
- Data link layer: framing, error detection (CRC), MAC protocols
- Network layer: IP addressing, subnetting, routing (distance vector, link state)
- Transport layer: TCP/UDP, congestion control, flow control
- Application layer: DNS, HTTP, email protocols

---

## 3. Approximate Weightage Trend (last several years — treat as a prioritization guide, not gospel; recompute yearly from actual PYQ tallies your agent does)

| Section | Typical share |
|---|---|
| Programming & Data Structures | High — usually top-2 |
| Algorithms | High |
| Engineering Mathematics | High (13 marks fixed-ish) |
| General Aptitude | Fixed 15 marks — highly scorable, low prep cost |
| Computer Networks | Medium-high |
| Operating Systems | Medium-high |
| Databases | Medium |
| Theory of Computation | Medium |
| Computer Organization & Architecture | Medium |
| Digital Logic | Low-medium |
| Compiler Design | Low, but rising in recent years — don't skip |

**Rule the agent should follow:** every week, re-derive this table from the actual PYQ set it has processed (count marks per topic across the last 10-15 papers) rather than trusting this static table forever — GATE examiner committees shift emphasis year to year.

---

## 4. PYQ Sources (agent should fetch/verify freshness each time, links can go stale)
- Official previous papers & answer keys: released each year on the official GATE website of that year's organizing IIT (search "GATE CS/IT question paper [year] official PDF")
- GATE Overflow (gateoverflow.in) — community-verified solutions with discussion threads, generally considered the most rigorous for CS
- NPTEL / IIT lecture material for concept depth on TOC, Compilers, COA
- Standard textbooks for concept anchoring, not full read-throughs: Cormen (Algorithms), Galvin (OS), Korth (DB), Tanenbaum/Forouzan (Networks), Kurose-Ross (Networks), Hopcroft-Ullman (TOC), Kai Hwang / Morris Mano (COA/Digital Logic)

**Anti-pattern to avoid:** treating PYQs as a question bank to memorize. GATE reuses *concepts* and *question styles*, rarely exact questions. The daily workflow (see CLAUDE.md) is built around explaining the underlying concept first, then showing why each PYQ tests it, then generalizing to "what a twisted version of this question would look like."

---

## 5. Two-Cycle Roadmap

### Cycle 1 — 3rd year, first-ever attempt (GATE 2027) — framed as a **calibration attempt**
This is your first GATE exam ever, so there's no prior score/percentile data to anchor to. Goal is not peak score — goal is: full syllabus exposure once, build the PYQ-explanation habit, and get real exam-hall data (time pressure, negative-marking discipline, which sections you personally lose time on) that a mock at home can't fully replicate. Treat the actual score as diagnostic input for Cycle 2, not a pass/fail judgment on yourself.

Rough phase split (adjust to actual months remaining before Feb 2027):
1. **Foundation pass** — one full pass over every section above, depth over speed, PYQ-anchored (this doc's section 2, in weightage order from section 3)
2. **Problem-solving pass** — timed topic-wise PYQ sets, 2-mark-question focus, start tracking accuracy by topic
3. **Full mocks + error log** — full-length timed mocks, maintain a running "why I got this wrong" log (concept gap vs silly mistake vs time pressure vs guessed-and-lost — these need different fixes)

### Cycle 2 — 4th year attempt (GATE 2028) — the real attempt
Starts from the Cycle 1 error log, not from zero. Weak topics (by accuracy %, not by feeling) get re-taught in depth first. Strong topics move straight to speed drills. Last 6-8 weeks: daily full/half mocks with strict negative-marking discipline and a live percentile-tracking log.

---

## 6. Score-to-percentile reality check
GATE CS is normalized and highly competitive — a 70-80 **percentile** is a materially different number from 70-80 **marks out of 100**; don't conflate them. Ask the agent to help you map your mock scores to historical percentile bands for GATE CS each time you take a full mock, using recent years' score-vs-percentile/rank data (search fresh each session — these tables move).
