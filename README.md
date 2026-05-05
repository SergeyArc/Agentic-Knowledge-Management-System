# Agentic Knowledge Management System

A minimalist, AI-native framework for Obsidian designed for high-velocity learning, research, and knowledge synthesis. 

Unlike traditional "Second Brains" that act as digital graveyards, this vault is built as a **computational engine** where you and an AI agent (Claude Code) collaborate to build, verify, and master knowledge.

Inspired by Andrej Karpathy's original [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) idea: use an LLM to compile raw sources into a persistent, interlinked wiki instead of re-deriving knowledge from scratch on every query.

## Key Philosophy

1. **Atomic Assertions**: Every note is a single claim you can prove or refute. Titles are assertions (e.g., "Attention mechanisms weight token importance"), not just topics.
2. **Agentic Workflows**: Use **Claude Code** as your "Chief Knowledge Officer" to ingest articles, detect contradictions, and run active recall sessions.
3. **Maps of Content (MOCs)**: No rigid folder hierarchies. Knowledge is organized via fractal indices (MOCs) that act as neural hubs.
4. **Integrated Mastery**: Spaced repetition and active recall are built directly into the note structure. No extra plugins required.

---

## Structure

```text
.
├── Inbox/          # Raw materials, articles, and unparsed logs.
├── Notes/          # Your personal "Atomic Knowledge Engine" (Zettels).
├── MOCs/           # Maps of Content. Fractal navigation and structural hubs.
├── Projects/       # Temporal research projects with specific end-goals.
├── Attachments/    # Media and PDFs.
├── MANIFEST.md     # The "Constitution" of the system.
└── CLAUDE.md       # The "Brain" – Instructions for your AI Agent.
```

---

## Metadata Standard (YAML)

The system relies on a strict but simple YAML schema to enable AI-agent automation:

| Field | Description |
| :--- | :--- |
| `note_type` | `permanent` (atomic knowledge), `literature` (source notes), `fleeting` (temporary thoughts), `structure` (MOC), or `project` (project files). |
| `status` | `stub` (unwritten), `red` (unlearned), `yellow` (familiar), `green` (mastered). |
| `up` | Wikilinks to parent MOCs (provides breadcrumbs). |
| `source` | Provenance link (URL, Book, or Zotero ID). |

---

## Supported Scenarios

This vault is optimized for working with an AI agent. Run `claude` in the root directory; the agent reads `CLAUDE.md` and follows the system rules.

- **Ingest new material**: turn articles, book chapters, transcripts, and raw notes from `Inbox/` into atomic notes.
- **Create permanent notes**: capture standalone theses with review questions, essence, provenance, and MOC links.
- **Run active recall**: review `red` and `yellow` notes and update mastery status through question-answer sessions.
- **Analyze knowledge gaps**: find orphan notes, stale stubs, empty MOC gaps, weak links, and overloaded MOCs.
- **Work on research projects**: initialize and run structured research projects in `Projects/`, with project maps, logs, decisions, section indexes, and links back to durable vault notes.
- **Synthesize answers**: combine existing notes into a cited answer and save valuable conclusions as new notes.
- **Run NotebookLM-style sessions**: ask targeted questions to external RAG tools, then convert useful answers into vault notes.

For detailed session flows and exact prompts, see [How to Work with the System Through the Agent](How%20to%20Work%20with%20the%20System%20Through%20the%20Agent.md).

---

## Setup & Installation

1. **Clone this Template:** Use this repository as a template to create your new vault.
2. **Open in Obsidian:** Point Obsidian to the cloned folder.
3. **Install Claude Code:** Ensure you have the [Anthropic Claude CLI](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code) installed.
4. **Start Thinking:**
   - Run `claude` in your terminal.
   - Say: *"Create a Global Index MOC and my first domain MOC for [your topic]."*

---

## 🚀 First 10 Minutes

Here's exactly what to say to your agent to go from zero to a working knowledge base.

**Step 1 — Initialize your vault structure (2 min)**

```markdown
Create a Global Index MOC in MOCs/ for my knowledge base.
My main domains are: [AI Engineering, ML, Philosophy].
Create a top-level MOC for each domain.
```

**Step 2 — Create your first atomic note (3 min)**

```markdown
I want to capture this idea as a permanent note:
"[Your idea or concept here]"
It belongs to the [domain] MOC.
Create it following the manifest template.
```

**Step 3 — Run your first gap analysis (2 min)**

```markdown
Open MOCs/[Your Domain].md and analyze the Gaps section.
What are the 3 most important concepts missing from this domain
that I should study first?
Add them as stubs linked to the MOC.
```

**Step 4 — Ingest your first source (3 min)**

```markdown
I have this article/note in Inbox/: [filename or paste text].
I'm particularly interested in [specific aspect].
Process it into atomic notes following the Ingest workflow.
```

After 10 minutes you will have: a working MOC structure,
your first permanent notes, visible knowledge gaps,
and a clear list of what to learn next.

---

## Manifesto
The core rules of this system are defined in `MANIFEST.md`. It covers:
- Why **Titles-as-Assertions** prevent the "Illusion of Competence."
- How **Fractal MOCs** allow the vault to scale to thousands of notes without friction.
- Why **Sourcing (Provenance)** is your insurance against AI hallucinations.

---

## Contributing
This is an evolving framework. If you've developed new Agentic Workflows or improved the `CLAUDE.md` instructions, feel free to submit a PR.

---

**License:** MIT. Build your brain, share the code.