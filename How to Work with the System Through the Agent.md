### Before Each Session

Open Claude Code at the root of the vault.
For example:

```
cd ~/Documents/Obsidian/common_vault
```

The agent automatically reads `CLAUDE.md` and knows the system rules. You do not need to explain anything else.

---

### Scenario 1: You Found Interesting Material

An article, a book chapter, a podcast transcript, or a video.

**What you do:** Put the file into `Inbox/`. You can add one line about what caught your attention.

**What you tell the agent:**

```
Process the file from Inbox: [file name].
I am especially interested in [aspect].
```

**What the agent does:** Asks a clarifying question -> atomizes the material -> creates notes in `Notes/` -> updates the MOC -> adds links.

**Your role in the process:** Answer clarifying questions. Accept or reject the proposed theses. The final decision is always yours.

---

### Scenario 2: A Thought or Term Comes Up

**What you do:** If you can formulate the thesis, tell the agent right away. If you do not have time to think, drop it into `Inbox/` as is.

**What you tell the agent:**

```
Create a note: "RAG solves the knowledge cutoff
more cheaply than fine-tuning". MOC — RAG Systems.
```

**What the agent does:** Creates a note from the template, sets `status: stub`, links it to the MOC, and suggests 2-3 links to existing notes.

---

### Scenario 3: You Want to Review a Topic

**What you tell the agent:**

```
Run a recall session for MOC RAG Systems.
Start with red, then yellow.
```

**How the session works:** The agent shows only the Question. You answer out loud or in writing. The agent shows the Essence. You assess the quality of your answer yourself and tell the agent the new status.

**Commands during the session:**

```
"red" / "yellow" / "green"  <- update status
"stop"                       <- end the session
"next"                       <- skip the note
```

---

### Scenario 4: You Want to Understand What You Know and Where the Gaps Are

**What you tell the agent:**

```
Run lint for MOC AI Engineering.
Show gaps and orphan notes.
```

**What you get:**

- Empty sections in the MOC are knowledge gaps
- Notes without links are lost notes
- Stubs older than a week are processing debt
- Overloaded MOCs are a signal for fractal splitting

---

### Scenario 5: You Ask a Question About Your Knowledge Base

**What you tell the agent:**

```
What do I know about attention mechanisms in transformers?
Use only notes from the vault.
```

**What the agent does:** Finds relevant MOCs -> reads linked notes -> answers with links to specific notes.

**After a good answer:**

```
Save this analysis as a note in Notes/.
```

Your questions and conclusions compound in the knowledge base just like sources do.

---

### Scenario 6: You Process the Entire Inbox

Every 2 days.

**What you tell the agent:**

```
Process Inbox. Show the list of files and suggest how to handle each one.
```

**For each file, the agent suggests one of three options:**

- Atomize -> create notes right now
- Clarify -> ask one question about context
- Postpone -> create a stub in `Notes/` if the topic is clear but there is no time

The Inbox should be empty after the session.

---

### Scenario 7: Collecting Knowledge on a Topic Through NotebookLM

Use this when you have a collection of materials in NotebookLM and want to quickly extract structured notes from them.

---

**Preparation (you do this yourself, without the agent):**

Gather all materials on the topic into one Notebook in NotebookLM: articles, PDFs, transcripts. Make sure the Notebook is loaded and can answer questions.

---

**What you tell the agent:**

```
I want to collect notes on [RAG Retrieval Strategies].
I have a NotebookLM with materials on this topic.
I will pass your questions to NotebookLM and return the answers.
Start with a MOC audit.
```

---

**How the session works:**

The agent first looks at the MOC for the topic and reports:

```
Agent: "I see 3 notes on RAG. Gaps in the MOC:
sparse vs dense retrieval, reranking, hybrid search.
I will ask about them. First question:..."
```

Then the cycle continues:

```
Agent asks a question
    |
You copy the question into NotebookLM
    |
NotebookLM answers
    |
You copy the answer back to the agent
    |
Agent analyzes it and asks the next question
```

---

**Your commands during the session:**

```
"stop"             <- interrupt the session
"enough"           <- move to note creation early
"clarify question" <- if the question is too broad
"skip"             <- if the topic is not a priority right now
```

---

**How the agent decides when to stop:**

The agent will say:

```
"There is enough information — all MOC gaps are closed
[or: the last two answers did not add anything new].
I am moving to note creation. Please confirm."
```

You answer "yes" — the agent starts the standard Ingest workflow.

---

**Important note about sources:**

If the agent formulates a thesis that seems inaccurate, ask NotebookLM "which source is this from?" and pass the quote to the agent. The agent will put it into the `source:` field in the note. This protects against hallucinations.

---

**Session result:**

The agent creates notes in `Notes/`, updates the MOC, and closes gaps using the standard Ingest workflow. You get structured knowledge from raw materials in a single session.

---

## Scenario 8: Working on a Research Project

**Project initialization (first session):**

```
You to the agent: "Starting a new project: [title].
Research question: [question].
Create the structure in Projects/[slug]/."
```

The agent creates:

- `README.md` with the map and instructions for the agent
- `log.md`
- `decisions.md`
- Subfolders for the sections you discussed

---

**Start of each following session:**

```
You to the agent: "Continuing project [title].
Today we are working on [section]."
```

The agent reads only `README.md` -> opens the `index.md` of the needed section -> asks what exactly you want to do.

---

**Work inside the session: three modes**

**Mode A: Research**

```
You: "Study this regulation and add it to the L1 ontology."
```

The agent reads the material -> suggests what to add to the file -> records it in `log.md`.

**Mode B: Synthesis**

```
You: "What do we know about the regulatory cycle
     across all three ontology layers?"
```

The agent reads the needed files sequentially -> synthesizes -> suggests saving the conclusion in `decisions.md` or as a note in the vault.

**Mode C: Decision**

```
You: "We need to decide how to model
     the medical device lifecycle."
```

The agent proposes a decision with links to sources -> you accept or correct it -> the agent records it in `decisions.md`:

```markdown
## Decision: lifecycle model

Date: YYYY-MM-DD
Context: [problem description]
Decision: [what was decided]
Rationale: [[note from vault]] + [link to regulation]
Alternatives: [what was rejected and why]
```

---

**When knowledge appears that is valuable on its own:**

The agent proposes it proactively:

```
Agent: "The concept 'regulatory cycle' is
atomic knowledge independent of the project.
Create a note in the vault and link it from here?"
```

You say "yes" — the agent creates a note in `Notes/`, updates the MOC, and adds a `[[link]]` to the project file.

---

**End of the session:**

The agent always ends the session the same way:

```
Agent: "Session complete. I recorded in log.md:
- what was done
- what remains
- the next step

Are there any vault insights we did not capture?"
```

The last question matters: this is the moment when knowledge from the project moves into the knowledge base.

---

**Commands while working on a project:**

```
"show project map"        <- README.md status table
"what did we do last time" <- latest log.md entry
"record decision"         <- add to decisions.md
"this goes to vault"      <- create a note from the current insight
"show gaps"               <- what is not closed in section index.md files
```

---

### How Not to Get Lost

**If you do not know where to start:**

```
Open Global Index.md and show
where I have the most gaps.
```

**If you have not opened the knowledge base for a while:**

```
Show all notes with status: stub
older than 7 days. Where should I start?
```

**If you want to see progress:**

```
How many notes do I have by status
in MOC AI Engineering?
```