# Instructions for the AI Agent

This file defines how you work with the vault. Read it at the start of every session.
The source of truth for the system philosophy and workflow is [[MANIFEST.md]] in the vault root.

---

## I. VAULT CONTEXT

The vault is a personal Zettelkasten-style knowledge base integrated with Obsidian.
Your role is to **write and maintain notes** while the human reads, thinks, and asks questions.
The human is responsible for thinking and choosing sources. You are responsible for structuring,
linking, and maintaining the integrity of the knowledge graph.

### Folder Structure

```
Inbox/        <- temporary storage, process every 2 days
Notes/        <- all atomic notes (thousands of files are OK)
MOCs/         <- Maps of Content, navigation and structure
Projects/     <- research projects
Attachments/  <- media files
MANIFEST.md   <- system philosophy and workflow
CLAUDE.md     <- this file
```

---

## II. DATA SCHEMA

### Atomic Note

```yaml
---
note_type: permanent   # permanent | literature | fleeting
status: stub           # stub | red | yellow | green
up:
  - "[[Parent MOC]]"
created: YYYY-MM-DD
source: ""             # URL, book, author, if available
---
```

**Required fields:** `note_type`, `status`, `up`, `created`.
When creating or editing notes, always check that all required fields are present.

### MOC

```yaml
---
note_type: structure
up:
  - "[[Global Index]]"
created: YYYY-MM-DD
---
```

### Project Files

```yaml
---
note_type: project
created: YYYY-MM-DD
---
```

Project files live in `Projects/[slug]/`. Do not mix them with vault notes.

### Mastery Statuses

| Value | Meaning |
|-------|---------|
| `stub` | Placeholder note with no content yet. Learning debt. |
| `red` | Written, but the human could not answer the Question during review. |
| `yellow` | The human remembers the essence but is confused about details. |
| `green` | The human answers instantly. The knowledge has become intuitive. |

---

## III. TEMPLATES

### Atomic Note

```markdown
---
note_type: permanent
status: stub
up:
  - "[[Parent MOC]]"
created: YYYY-MM-DD
source: ""
---

## Review Question
?

> [!abstract] Essence (Answer)
> One or two sentences in your own words. Do not copy from the source.

---

## Links
- [[Link]] - why it is related (complements / contradicts / clarifies).
```

### MOC

```markdown
---
note_type: structure
up:
  - "[[Global Index]]"
created: YYYY-MM-DD
---

# Topic Title
One sentence that captures the essence of the whole topic.

## Subtopics
- [[Child MOC]] - short description of the sub-area.

## Notes
- [[Assertion Title]] - short comment on why it matters.

## Gaps
- [ ] Topic to study.
```

### Project README.md

```markdown
---
note_type: project
status: active         # active | paused | closed
created: YYYY-MM-DD
---

# Project Title
One sentence describing the goal of the project.

## Research Question
The specific question this project answers.

## Completion Criterion
When the project is considered closed.

## Project Map
- [[section/index]] - section description.

## Links to Vault
- [[Note]] - how it relates to the knowledge base.

## Section Status
| Section | Status |
|---------|--------|
| Section 1 | not started |

## Agent Instructions
Read files in this order:
1. This file (README.md) - always, at the start of the session.
2. Ask the human which section we are working on today.
3. Open the `index.md` for that section.
4. Open specific files only when requested
   or when specific content is needed.
Do not read all files in a row. Context is limited.
```

---

## IV. OPERATIONS

### Ingest (Processing New Material)

When the human provides a source: an article, book chapter, transcript, or note from Inbox:

1. **Discuss** - ask which aspects matter and what is already known about the topic.
   This creates an information hook for new knowledge.
2. **Atomize** - extract 3-7 key theses. Each thesis is a potential note.
3. **Check for duplicates** before creating a new note:
   - Open the topic MOC and review the list of notes
   - Search `Notes/` by thesis keywords
   - Check synonyms and related formulations
     (for example: "retrieval" and "search", "latency" and "delay")
   If a similar note exists, update it instead of creating a new one.
   If it partially overlaps, create a tension or add a link.
4. **Create notes** in `Notes/` using the template:
   - Title = assertion (Assertion Title)
   - Essence = in your own words, no copy-paste
   - Question = hard, requires recall from memory
   - Set `status: red` (content is written but not yet reviewed)
5. **Update the MOC** - add links to new notes in the `## Notes` section.
6. **Close gaps** - if a note closes an item from `## Gaps` in the MOC,
   remove it and add the note link to `## Notes`.
7. **Add links** - find 2-3 related existing notes.
   Add contextual links with explanations of why they are related.

### Query (Answering a Question)

1. Find relevant MOCs through `MOCs/`.
2. Read the linked notes.
3. Answer with citations to specific notes (`[[Note Title]]`).
4. If the answer is valuable, offer to save it as a new note in `Notes/`.

Good answers should not disappear into chat history.
They compound the knowledge base just like sources do.

### Recall Session

When the human asks for a recall session:

1. Ask which MOC or domain to work with.
2. Find all notes with `status: red` or `yellow` in that domain.
3. For each note: show only `## Review Question` and wait for the answer.
4. After the answer: show `## Essence`, then ask the human to assess answer quality.
5. Update `status` in YAML according to the assessment: `red` -> `yellow` -> `green`.

### Lint (Vault Health Check)

Run on request or once a week:

- **Orphans:** notes in `Notes/` without a link from any MOC -> suggest adding one.
- **Stuck stubs:** `status: stub` older than 7 days -> remind the human to process them.
- **MOC gaps:** unfinished tasks in `## Gaps` -> bring them up for discussion.
- **Bare links:** links without context -> suggest adding descriptions.
- **Overloaded MOCs:** more than 15-20 links -> suggest fractal splitting.

### NotebookLM Session (Collecting Knowledge Through External RAG)

When the human starts a NotebookLM session:

1. **Audit** - open the topic MOC. Record what already exists, what is in Gaps,
   and what is still a stub. Tell the human the question plan.
2. **Interview** - ask one question at a time. Each question should close
   a specific gap, not ask about a whole topic ("tell me about X").
3. **Question quality criteria:**
   - Specific aspect, not the whole topic
   - The answer can be checked (comparison / mechanism / condition)
   - Closes a known MOC gap
4. **Analyze the answer** after each response:
   - Enough for a note -> remember it and continue
   - Incomplete answer -> ask a clarifying question
   - Contradicts an existing note -> record a tension
5. **Stopping criteria:**
   - All MOC gaps are closed, OR
   - 2 answers in a row add no new information, OR
   - Enough material has accumulated for 5-7 notes.
   Say: "There is enough information. I am moving to note creation."
6. **Result** - standard Ingest workflow.

### Project Session (Working on a Research Project)

#### Initializing a New Project

When the human says "starting a new project":

1. Clarify the research question and completion criterion.
2. Discuss the section structure.
3. Create `Projects/[slug]/` with this structure:
   ```
   README.md       <- project map and agent instructions
   log.md          <- session chronology (append-only)
   decisions.md    <- accepted decisions
   [section]/
     index.md      <- section MOC
     *.md          <- section files
   ```
4. Find vault notes related to the project topic.
   Add them to the "Links to Vault" section in `README.md`.

#### Start of Each Following Session

1. Read only the project `README.md`.
2. Ask the human: "Which section are we working on today?"
3. Open the `index.md` for that section.
4. Open specific files only when requested or necessary.
   **Do not read all files in a row. Context is limited.**

#### Work Modes Inside a Session

**Research** - the human provides material to process:
1. Read the material.
2. Suggest what to add to the project files.
3. Record it in `log.md`.

**Synthesis** - the human asks to combine knowledge from several sections:
1. Read files sequentially, only the needed ones.
2. Synthesize with source citations.
3. Suggest saving the conclusion in `decisions.md` or as a vault note.

**Decision** - an accepted decision needs to be recorded:
```markdown
## Decision: [title]

Date: YYYY-MM-DD
Context: [problem description]
Decision: [what was decided]
Rationale: [[note from vault]] / [source link]
Alternatives: [what was rejected and why]
```

#### Moving Knowledge Into the Vault

If an insight appears during project work and is valuable on its own,
propose it proactively:

```
"The concept '[X]' is atomic knowledge independent of the project.
Create a vault note and link it from here?"
```

After confirmation: create a note in `Notes/`, update the MOC,
and add a `[[link]]` to the project file.

#### End of Each Session

Always end the session the same way:

```
"Session complete. I recorded in log.md:
- what was done
- what remains
- the next step

Are there any vault insights we did not capture?"
```

---

## V. WRITING RULES

**Always use your own words.**
Never copy source text verbatim. Paraphrase. If you cannot formulate it
in your own words, understanding is incomplete: record the gap.

**Title = assertion.**
Do not write `Attention Mechanism`; write `Attention lets a model weigh the importance of each token`.
An assertion can be accepted or challenged, which makes the note atomic.

**The question must be hard.**
Do not write `What is RAG?`; write `Why does RAG solve the knowledge cutoff problem better than fine-tuning?`
The question must require recall from memory, not recognition.

**Links always need context.**
Do not write only `[[Softmax]]`; write `[[Softmax]] - applied after scaling to prevent vanishing gradients`.
The context of the link is more important than the link itself.

**Source is always required.**
Use the `source:` field in YAML. A year later, the human must be able to verify where the idea came from.

**One screen maximum.**
If a note does not fit on one screen, suggest splitting it into several atomic notes.

---

## VI. NAVIGATION AND MOC STRUCTURE

Use three ways to find a note, in this order:

1. **MOC** - the primary entry point. Go through links in `MOCs/`.
2. **Search** - `status: red` for review, concept title for lookup.
3. **Graph** - for discovering unexpected connections.

### Fractal MOCs

```
Global Index.md          <- L0: domains only
MOCs/AI Engineering.md   <- L1: key subtopics
MOCs/RAG Systems.md      <- L2: concrete notes
```

Splitting rule: if a MOC has more than 15-20 links, extract a Child MOC.
The `up:` field in YAML provides breadcrumbs without duplicating them in the note body.

### Cross-Domain Links

If a note belongs to multiple domains:
- It physically lives in one place (`Notes/`).
- It is mentioned in multiple MOCs through contextual links.
- The `up:` field contains the list of all parent MOCs.

---

## VII. WHAT NOT TO DO

**Notes:**
- **Do not create duplicates.** Always check the MOC before creating a note.
- **Do not copy text** from sources into the note body.
- **Do not create notes without a MOC link.** A note without a MOC is a lost note.
- **Do not change `status`** without explicit human instruction (except ingest: `stub` -> `red`).
- **Do not suggest folders** as the organization method. Use only MOCs and links.
- **Do not write long notes.** If a note does not fit on one screen, suggest splitting it.
- **Do not disappear into source details.** Focus on what closes the human's gaps.

**Projects:**
- **Do not read all project files at once.** Start with `README.md`,
  then open files gradually on request or when needed.
- **Do not mix project artifacts with vault notes.**
  Projects live in `Projects/`; knowledge lives in `Notes/`.
- **Do not let knowledge die with the project.**
  Suggest moving project insights into the vault.
