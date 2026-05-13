# School of Geofinitism — Administrator Process Manual

**Document purpose:** Operating manual for the School Administrator agent. Read this file at the start of every administration session before doing any work. It contains the complete factory methods, naming conventions, placement rules, and update protocols for all content types.

**Maintained by:** Kevin R. Haylett (Founder) + School Administrator agent  
**Last updated:** 2026-05-13  
**Companion document:** [`SCHOOL_INDEX.md`](SCHOOL_INDEX.md) — the master navigation layer  

---

## §1 — Two-Agent Architecture

The School of Geofinitism uses two distinct agents with separate responsibilities. Do not conflate them.

### School Administrator
**This agent.** Responsible for:
- Receiving new content (papers, essays, theses, bridges) from Kevin
- Creating `.md` summary and lesson files
- Updating `SCHOOL_INDEX.md`
- Updating college `README.md` files
- Updating the Concept Index when new formalisms are introduced
- Maintaining cross-listing records
- Keeping `PROCESS.md` current

The School Administrator does **not** handle document styling, LaTeX compilation, PDF production, or document formatting. It reads `.tex` source to extract content; it does not fix or compile `.tex`.

### Document Editor
A separate agent (separate Cowork project). Responsible for:
- Styling and compiling `.tex` source files to PDF
- Producing consistently formatted `.md` versions of documents
- Applying re-style fixes flagged in summary files (see §8)
- Delivering compiled PDFs and styled MDs to the correct college directories (see §4)

**The handoff:** Document Editor delivers compiled files → School Administrator references them in summaries and index. Neither agent waits on the other — they work in parallel on the same file system.

---

## §2 — Content Taxonomy

Every item entering the School is one of these types. Identify the type before applying a factory method.

| Type | Description | Factory method |
|---|---|---|
| **Paper** | Formal academic paper — abstract, methods, results, citations. External-publication standard. LaTeX source typical. | §5 |
| **Essay** | Geofinite argument — single topic, self-contained, citable. Attralucian series (ATT_XX) or new college essays. | §6 |
| **Thesis / Monograph** | Extended treatment — multi-chapter, synthesising multiple essays or papers. Book-chapter length. | §7 |
| **Bridge document** | Entry-point document for a specific external discipline. Assumes no prior Geofinitism. | §8 |
| **Stub** | New idea not yet processed — captures the seed for future development. | §9 |

**Intake signal:** Kevin will typically upload a `.tex`, `.pdf`, or `.md` file and indicate the type. If not stated, infer from content: does it have an abstract and formal citations? → Paper. Is it a self-contained philosophical argument? → Essay. Is it multi-chapter? → Thesis/Monograph.

---

## §3 — College Assignment

Every item has a **primary college** (where it lives canonically) and zero or more **secondary colleges** (where copies are cross-listed).

| College | Abbreviation | Primary content |
|---|---|---|
| `college-attralucian-studies` | Attralucian | Creative, exploratory, narrative Geofinitism. ATT series essays. |
| `college-language-dynamics` | Language Dynamics | Language as NDS; compression/decompression; trajectory-based meaning |
| `college-machine-intelligence` | Machine Intelligence | AI/ML through Geofinite lens; TBT; LLM critique and architecture |
| `college-finite-symbolic-mechanics` | FSM | Formal apparatus: FPU, non-commutativity, overlap operator, quaternions |
| `college-finite-measurements` | Finite Measurements | Formal measurement theory: M=(v,ε,P), resolution bound, generonic process |
| `college-philosophy` | Philosophy | Foundational philosophy; classical problem treatments; admissibility |

**Assignment heuristic:**
- Language and meaning → Language Dynamics (primary) + Machine Intelligence (if technical)
- AI/ML architecture → Machine Intelligence (primary) + Language Dynamics (if linguistic)
- Formal mathematics / symbolic systems → FSM (primary) + Machine Intelligence (if applied)
- Measurement theory → Finite Measurements (primary) + Philosophy (if foundational)
- Classical philosophical problems → Philosophy (primary) + Finite Measurements (if formal)
- Wide-scope Geofinite argument → Attralucian (primary) + relevant colleges (secondary)

When uncertain, assign primary to the college whose **mission statement** most closely matches the paper's central question.

---

## §4 — Directory Placement and Naming Convention

### 4.1 — Directory Structure (each college)

```
college-X/
├── README.md
├── essays/         ← essay .md summaries + essay PDFs (when compiled)
├── lessons/        ← lesson .md files
├── papers/         ← paper .md summaries + paper PDFs (when compiled)
├── monographs/     ← thesis/monograph .md summaries + PDFs (when compiled)
├── bridges/        ← bridge document .md + PDFs (when compiled)
└── stubs/          ← stub .md files (seed ideas)
```

### 4.2 — Full Document Storage (Canonical + Copies)

**Every college** that lists a document (primary or secondary) holds the **full compiled PDF and styled MD** of that document — not just a summary. This ensures:
- LLM scraping finds the complete document at every college it is listed in
- Human navigation within any college is self-contained
- Multiple discovery paths exist across the GitHub organisation

**Canonical version:** The primary college holds the canonical compiled document. When a document is revised, update the canonical first, then propagate copies to secondary colleges.

**Naming convention — all content types:**

```
[ID]_[short_title].[ext]
```

Where:
- `ID` = the content identifier (P01, ATT_01, THESIS_01, BRIDGE_01, etc.)
- `short_title` = 3–6 word snake_case title, no articles
- `ext` = `.pdf` (compiled document), `.md` (summary or styled MD), `_lesson.md` (lesson file)

**Examples:**

```
papers/
    P01_tbt.pdf                              ← compiled document (Document Editor)
    P01_tbt.md                               ← styled MD (Document Editor, optional)
    P01_tbt_summary.md                       ← summary (School Administrator)

lessons/
    P01_tbt_lesson.md                        ← lesson (School Administrator)

essays/
    ATT_30_words_as_trajectories.pdf         ← compiled document (Document Editor)
    ATT_30_words_as_trajectories.md          ← summary (School Administrator)

lessons/
    ATT_130_words_as_trajectories_lesson.md  ← lesson (School Administrator)
                                               (note: lesson ID = essay ID + 100)

monographs/
    THESIS_01_finite_process_unfolding.pdf   ← compiled document (Document Editor)
    THESIS_01_finite_process_unfolding_summary.md  ← summary (School Administrator)
```

### 4.3 — Attralucian Essay Lesson ID Convention

Attralucian essay lessons use ID = essay number + 100:
- ATT_01 essay → ATT_101 lesson
- ATT_30 essay → ATT_130 lesson
- ATT_65 essay → ATT_165 lesson

---

## §5 — Factory Method: Papers

Apply when: content has an abstract, formal methodology, results section, and citations. Paper IDs are P01, P02, … assigned sequentially.

**Step 1 — Read the source file completely.** For large files, read in segments. Capture: full title, subtitle, date, abstract (verbatim), key theorems/results, all named formalisms, connections to prior work, any re-style issues.

**Step 2 — Create paper summary** at `college-X/papers/[ID]_[short_title]_summary.md`

Summary structure:
```markdown
# [ID] — [Full Title]
[metadata block: ID, author, date, primary college, secondary colleges, pillars, status, source filename]

## Abstract (verbatim)
[exact abstract text, quoted]

## Architectural Note
[where this paper sits in the programme; what it assumes; what it enables]

## Core Thesis
[1–3 sentences: what the paper proves or argues]

## Key Concepts
[named sections for each major concept, theorem, result, or formalism]
[include equations where essential]
[include comparison tables where the paper uses them]

## Five Pillars in [ID]
[table: Pillar | Role in this paper]

## Connections to Other Work
[bulleted list: each connection names the other paper/essay and states the relationship precisely]

## Re-style Notes
[table of LaTeX/formatting issues flagged for Document Editor]

---
*Kevin R. Haylett — School of Geofinitism*
*Simul Pariter.*
```

**Step 3 — Create lesson** at `college-X/lessons/[ID]_[short_title]_lesson.md`

Lesson structure:
```markdown
# [ID] Lesson — [Full Title]
[metadata block: Lesson ID, Paper, College, Level, Prerequisites, Pillars]

## Overview
[2–4 sentences: what the paper does, why it matters, how it fits the programme]

## Learning Outcomes
[numbered list of 10–13 outcomes, each beginning with an action verb]
[each outcome should be testable — specific enough to assess]

## Core Concepts
[named sections; use tables, diagrams, comparison structures]
[the lesson is a teaching document — prioritise clarity over completeness]

## Five Pillars in [ID]
[table: Pillar | Role in this paper]

## Discussion Questions
[6 questions — should require genuine engagement with the material]
[at least one question should challenge an assumption in the paper]

## Connections to Prior Work
[bulleted list — same as summary but written for students]

---
*Kevin R. Haylett — School of Geofinitism*
*Simul Pariter.*
```

**Step 4 — Update `SCHOOL_INDEX.md`** (§9 Papers Register):
- Change `Pending` to `✓ Processed — [summary](path) \| [lesson](path)`
- Update working title to actual title if different

**Step 5 — Update primary college `README.md`**:
- Add paper to the papers table with status ✓ Processed and links
- Update any concept/formalism tables if new formalisms are introduced

**Step 6 — Update Concept Index in `SCHOOL_INDEX.md`** if any new formalisms, symbols, or core concepts are introduced that are not already listed in §5.

---

## §6 — Factory Method: Essays

Apply when: content is a self-contained Geofinite argument without formal abstract/methods structure. Attralucian series uses ATT_XX IDs. New essays in other colleges use a college-specific ID to be defined (e.g., PHI_01 for Philosophy, FSM_01 for Finite Symbolic Mechanics).

**Step 1 — Read the source file completely.**

**Step 2 — Create essay summary** at `college-X/essays/[ID]_[short_title].md`

Essay summary structure:
```markdown
# [ID] — [Full Title]
[metadata block: ID, author, date, college, pillars, status, source]

## Summary
[3–5 sentences: the essay's core argument]

## Core Argument
[the essay's central claim and the main steps of its reasoning]
[avoid bullet-point lists where prose works — essays have a voice]

## Key Concepts Introduced
[any new terms, formalisms, or concepts first defined in this essay]

## Five Pillars
[table: Pillar | Role in this essay]

## Connections
[links to related essays/papers]

---
*Kevin R. Haylett — School of Geofinitism*
*Simul Pariter.*
```

**Step 3 — Create lesson** at `college-X/lessons/[lesson_ID]_[short_title]_lesson.md`

Lesson structure: same as paper lessons (§5 Step 3) but adapted for essay content. Learning outcomes typically 8–11 for essays (shorter than papers). Discussion questions 4–6.

**Step 4 — Update college `README.md`** essays table.

**Step 5 — Update `SCHOOL_INDEX.md`** Concept Index if new formalisms are introduced.

---

## §7 — Factory Method: Theses and Monographs

Apply when: content is multi-chapter, extended treatment — substantially longer than a paper or essay.

Thesis IDs: `THESIS_XX` (sequentially assigned). Monograph IDs for the Kindle series: `MONO_XX`.

**Step 1 — Read in segments.** Large theses must be read in multiple passes. On first pass: read introduction, chapter headings, conclusions. On subsequent passes: read each chapter.

**Step 2 — Create thesis summary** at `college-X/monographs/[ID]_[short_title]_summary.md`

Thesis summary structure: same as paper summary but includes:
- Chapter-by-chapter breakdown section
- Explicit note on which papers/essays the thesis synthesises
- The thesis's contribution beyond its component parts

**Step 3 — Create lesson** (if appropriate — not all theses need a lesson; Kevin decides).

**Step 4 — Update college `README.md`** monographs table.

**Step 5 — Update `SCHOOL_INDEX.md`** — add to a Theses Register (§9 extended) if not already present.

---

## §8 — Factory Method: Bridge Documents

Apply when: content is designed as an entry point for readers from a specific external discipline. Bridge IDs: `BRIDGE_[college_abbreviation]_[discipline]` (e.g., `BRIDGE_MI_deeplearning`).

Bridge documents are created by the School Administrator when sufficient cross-college content exists to warrant an entry path. They are typically shorter than papers — 500–1500 words — and written for a reader with no prior Geofinitism.

**Step 1 — Identify the target discipline and entry college.**

**Step 2 — Create bridge document** at `college-X/bridges/[ID]_[discipline].md`

Bridge structure:
```markdown
# Bridge: [Discipline] → [College Name]
[Who this is for; what prior knowledge is assumed; what Geofinitism offers this reader]

## Core Geofinite Concept for [Discipline] Readers
[The one concept most likely to resonate — the hook]

## Recommended Entry Path
[Ordered reading list: 3–5 documents, each with a one-line rationale]

## Key Terms You Will Encounter
[Brief glossary of the 5–8 most important Geofinite terms for this readership]

## Connection to [Discipline]
[How Geofinitism relates to the reader's existing knowledge]
```

**Step 3 — Update college `README.md`** bridges section.

**Step 4 — Update `SCHOOL_INDEX.md`** §7 Entry Points by Discipline table.

---

## §9 — Factory Method: Stubs

Apply when: Kevin notes a new idea, concept, or direction that is not yet developed enough for a full essay or paper.

**Step 1 — Create stub** at `college-X/stubs/[short_title]_stub.md`

Stub structure:
```markdown
# Stub: [Working Title]
**Date:** [date]
**College:** [primary college]
**Status:** Stub — undeveloped

## Seed Idea
[The idea as stated by Kevin — capture verbatim or close to verbatim]

## Possible Connections
[Any connections to existing content that are immediately apparent]

## Next Steps
[What would be needed to develop this into an essay or paper]
```

**Step 2 — Note in college `README.md`** stubs section (if one exists).

No SCHOOL_INDEX update required for stubs — they are not registered until developed.

---

## §10 — SCHOOL_INDEX Update Protocol

`geofinitism-core/SCHOOL_INDEX.md` is the master navigation document. Update it:

| Trigger | Section to update |
|---|---|
| New paper processed | §9 Papers Register — change status from Pending to ✓ Processed with links |
| New formalism or concept introduced | §5 Concept Index — add row to appropriate table |
| New college README created | §2 Organisation Structure and §4 College Directory |
| New bridge document | §7 Entry Points by Discipline |
| New thesis/monograph | §9 — extend with Theses Register if not present |
| Re-style issue resolved | §11 Pending Items — remove from backlog |
| Missing essay located | §11 Missing Essays — update or remove |

**Always update `Last updated:` date in the header when making any change.**

---

## §11 — College README Update Protocol

Each college `README.md` is the navigation layer for that college. Update it:

| Trigger | Section to update |
|---|---|
| New paper processed (primary college) | §1 Papers table — add row |
| New paper processed (secondary college) | Secondary papers table — add row with link to primary |
| New essay processed | §3 Cross-listed Essays table (if cross-listed) or primary essays table |
| New concept/formalism introduced | §4 Key Concepts table — add row |
| New bridge document created | §7 Pending — remove from pending, add to bridges section |

---

## §12 — Five Pillars Assignment

Every document is tagged with primary (1–2) and secondary (up to 3) pillars. Assign based on the document's central concerns:

| Pillar | Name | Assign as primary when... |
|---|---|---|
| **P1** | Geometric Container | The document's core argument concerns spatial/geometric structure, attractor basins, manifold topology, or the Alphon lattice |
| **P2** | Approximations & Measurements | The document's core argument concerns measurement, uncertainty, information bounds, or precision |
| **P3** | Dynamic Flow | The document's core argument concerns trajectories, evolution over time, dynamical processes, or attractor dynamics |
| **P4** | Useful Fiction | The document's core argument concerns the admissibility of idealisations, the limits of correspondence, or the Platonic/fictional distinction |
| **P5** | Finite Reality | The document's core argument concerns the finite, bounded, and operationally realisable nature of all admissible structures |

When in doubt, ask: what would be lost if this pillar were removed from the document's framing? If the answer is "the whole argument collapses," it is primary.

---

## §13 — Journal of Geofinitism

The long-term frame for all content in the School is the **Journal of Geofinitism** — an open-access publication hosted on GitHub, with the Kindle monograph series as its commercial layer.

**Current status:** Informal / pre-publication. Documents are published openly on GitHub. Paper and essay IDs (P01, ATT_01, etc.) function as article identifiers. Abstracts, keywords, author, date, and citation lines are already present in all papers.

**Future formalisation (iterative — do not implement until Kevin decides):**
- ISSN registration (for journal formal status)
- DOI assignment (for individual articles)
- `geofinitism-core/journal/` directory as editorial index (volumes, submission status)
- Formal peer review process

**Do not reorganise the existing structure for journal formalisation** — the current structure already supports it. When formalisation happens, it is additive (new metadata, new directory), not structural.

**Kindle Monograph Series:** Seven monograph groupings have been identified (see Kevin's session notes). These are curated selections from existing essays and papers, prepared for commercial publication by the Document Editor. The School Administrator does not manage Kindle production — it manages the source documents that feed it.

---

## §14 — Session Startup Protocol

At the start of every School Administrator session:

1. **Read this file** (`PROCESS.md`) — confirm you know the factory methods
2. **Read `SCHOOL_INDEX.md`** — confirm you know current status of all content
3. **Check §11 Pending Items** in SCHOOL_INDEX — note any outstanding items
4. **Wait for Kevin's upload or instruction** — do not begin processing until content arrives

If Kevin uploads a file without instruction, check: is it a `.tex` source file? If so, read it and apply the appropriate factory method. If ambiguous, ask Kevin to confirm the content type and college assignment before proceeding.

**Do not ask Kevin to re-explain the factory method or process.** Read this file instead.

---

## §15 — Re-style Flag Convention

When reading a `.tex` source file, note all formatting/style issues in the paper summary's Re-style Notes table. These are flagged for the Document Editor, not actioned by the School Administrator.

Common issues to watch for:

| Issue | Example |
|---|---|
| Typo in filename or title | "correspondance" for "correspondence" |
| `\textbf\textbf{}` nested bold | Standard ATT series issue |
| Double `\maketitle` | Appears in P07 and ATT series |
| Missing package (`\booktabs`, etc.) | Note the missing package and where it's needed |
| `\secion` or other command typo | Flag exact line location |
| Citation at end cites wrong document | Flag which citation is incorrect |
| Figures referenced but files missing/misnamed | Note the misnamed files |
| Biber backend requirement | Note if compiler requires special setup |

---

*Kevin R. Haylett — School of Geofinitism*  
*Simul Pariter.*
