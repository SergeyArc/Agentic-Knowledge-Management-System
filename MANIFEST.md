# SYSTEM MANIFEST: LLM Wiki & Self-Study

## I. CORE PRINCIPLES
1.  **Atomicity (1 note = 1 thesis):** The note title is an assertion (Assertion Title). If a note contains more than one idea, split it.
2.  **Navigation through MOCs:** Folders are physical containers. Meaning lives in Maps of Content (MOCs). A note without a link from a MOC is considered lost.
3.  **Active Recall:** Every note is a micro-exam. We do not reread the Essence; we answer the Question.

---
## II. TEMPLATES (Strict Typing)

### 1. Atomic Note Template (in `Notes/`)
```markdown
---
note_type: permanent
status: stub
up:
  - "[[Parent MOC]]"
created: 2026-04-21
---

## Review Question
?

> [!abstract] Essence (Answer)
> Answer text that can be hidden or expanded.

---

## Links
- [[Link]] - description of the link context.
```

### 2. Index Template (in `MOCs/`)
```markdown
---
note_type: structure
up:
  - "[[Global Index]]"
---

# Topic Title
One sentence that captures the essence of the whole topic.

## Subtopics (Lower Level)
- [[Child MOC]] - short description of the sub-area.

## Notes (Zettels)
- [[Note Assertion Title]] - short comment.

## Gaps
- [ ] Topic to study (turns into a link in "Notes" after creation).
```

---
## III. MASTERY STATUSES
*Status is recorded only in the YAML `status:` field*

*   **stub:** Placeholder note. No content yet. Your learning debt.
*   **red:** Content is written, but you could not answer the Question. Needs review.
*   **yellow:** You remember the essence but are confused about details. Needs reinforcement.
*   **green:** You answer instantly. The knowledge has become part of intuition.

---

## IV. WORKFLOW

### 1. Capture: Quick vs Raw
*   **Quick capture (Directly in `Notes/`):** If you immediately understand the essence and can formulate the thesis, create the note directly in `Notes/`, set `status: stub`, and link it to a MOC.
*   **Raw capture (In `Inbox/`):** If you found an article, long text, or collection of links that you do not have time to process, put it into `Inbox/`. This is raw material that has not yet become your knowledge.

### 2. Processing Inbox
Clear `Inbox/` every 2 days:
1.  Read the material.
2.  Extract 3-5 key theses from it.
3.  Create a separate note in `Notes/` for each thesis using the template.
4.  Delete the source file from `Inbox/` or move it to an archive outside the system.

### 3. Gap Lifecycle
*   The `## Gaps` section in a MOC is your learning plan.
*   **Conversion rule:** As soon as you create a note for a topic from the gaps list:
    1.  Remove the line from `## Gaps`.
    2.  Add a link to the created note in `## Notes`.

### 4. Distillation
*   Expand the `stub`. Formulate the thesis in the title, write the Essence and the Question.
*   Update the status to `red`.

### 5. Recall
*   **Daily Recall:** Open Obsidian search and enter `status: red` or `yellow`; this is today's list.
*   **Session:** Read the Question -> answer out loud -> expand the callout -> compare.
*   **Update:** Change the YAML status based on the result.

---

## V. RULES FOR THE AI AGENT (CLAUDE.md)
*   **Schema Validation:** Always check that `note_type` and `status` are present in YAML.
*   **Inbox Logic:** If the user asks to "save an article", suggest `Inbox/`. If they ask to "make a note", use `Notes/`.
*   **Assertion Titles:** When generating titles, use assertion form.
*   **Gap Management:** Automatically move notes from MOC checkboxes into the MOC when they are created.

---

## VI. FOLDER STRUCTURE (Logistics)
*   `Inbox/` - **Temporary storage.** External texts, articles, raw materials. Should be empty by the end of the week.
*   `Notes/` - **Database engine.** Your personal atomic assertions. Only verified, processed content.
*   `MOCs/` - **Map of meanings.** Structure and navigation.
*   `Attachments/` - Media files.

---

## VII. PROJECTS

A project is a temporary artifact with a goal and completion criterion.
It lives in `Projects/[slug]/`. It gets closed. Notes are permanent.

**Two directions of linking:**
- Project -> vault: insights from the project become notes.
- Vault -> project: notes are used as rationale for decisions.

Knowledge must not die with the project.