---
description: Extract structured information from tailored resumes, PDFs, or code into knowledge base extractions
---

# /setup-extract

**User input:** `$ARGUMENTS`

Parse `$ARGUMENTS`:
- Folder path to tailored resumes (e.g., `industry/Smith2024_catalyst.pdf`, `industry/project_report.tex`) → read that file
- Empty → ask the user for the resume folder or paste content

---

## Startup

1. Read `CLAUDE.md` — check KB Corrections Log for known issues
2. Read `config.md` — load Personal Info (to identify user's author position), Provenance Flags
3. Read `knowledge_base/extractions/_INVENTORY_INDUSTRY.md` — see what's already extracted, avoid duplicates

If any filename (Resume) is already in the inventory:
- Skip the file and tell the user that [filename] is already extracted

---

## Phase 1: Read & Understand all the Resumes

1. List all PDF files in folder given in the argument. Use the Read tool (supports PDF reading)
2. **While reading, for each do collect:**
    1. Skills (organized by: Programming Languages, Frameworks & Libraries, Cloud & Infrastructure, Databases, DevOps & CI/CD, Observability & Monitoring, Security, Testing, Messaging & Streaming, AI/ML, Methodologies & Practices, Tools)
    2. Projects (all projects with full bullet point details)
    3. Volunteer Experiences
    4. Role Types Targeted (Would be found at the top of the resume file)
    5. Key Themes & Strengths (top recurring themes with explanations)
    6. Metrics & Quantified Achievements (consolidated table with averaged numbers)
    7. Preferred Industries / Domains
    8. Job Matching Notes (keywords by category, seniority signal, soft skill differentiators, geographic/logistics notes)
3. Move to the next step when all of the files have been read
Progress: "Reading resume... [Tailored Job Title]"

---

## Phase 2: Clarify Each Tailored Resume

**Questions to ask (skip any that are already clear from the resume):**
1. "What Job Title is this resume tailored for?" (e.g., Software Engineer III)
2. "What was your specific contribution at [Company]? (e.g., What did you do at Abc Corp)"
3. "Did you develop any tools, methods, or code at this [Company] [Position]?"
4. "Any quantitative results you can personally claim? (e.g., 'I ran all the simulations')"
5. "Is there anything in this [file name] that should NOT appear on your knowledge base? (e.g., Specific Technologies you're not confident about)"

### >>>>>> MANDATORY STOP — DO NOT PROCEED <<<<<<
Present your understanding of each tailored in few sentences resume one by one. 
Ask the clarifying questions for ALL papers at once (grouped).
**You MUST wait for the user's explicit text response before continuing to the next resume.**

---

## Phase 3: Write Extraction

Create the extraction file at `knowledge_base/extractions/resume_context_<date>.md`

**Naming convention:** `resume_context_<date>.md`
- Examples: `resume_context_march_16_2026.md`
- Normalize to lowercase with underscores

**Extraction format:**

```markdown

## Resume Bullet Seeds
Applied to all the bullets in all the sections below
[3-5 draft bullets in STAR format. These are seeds, not final text.]
[Use full-ownership verbs only for sole-contributor work. Hedge for shared work.]
1. [Action verb] + [what was done] + [quantitative result/impact]
2. [Action verb] + [method/tool developed] + [what it enabled]
3. [Action verb] + [scope — e.g., "across N systems"] + [outcome]
4. [Optional: collaboration-framed bullet]
5. [Optional: tool/infrastructure bullet]

## Sections (in order)

- Professional Summary
- Positioning core value prop; role positioning table by resume type in format:
| Resume Type / Target Role | Key Angle |
|---|---|
- Work Experience (full details per role: company, title, location, dates, ALL bullet points with averaged metrics noted)
- Education (degrees, institutions, dates, GPA, relevant coursework, capstone projects)
- Publications
- Tools (organized by: Programming Languages, Frameworks & Libraries, Cloud & Infrastructure, Databases, DevOps & CI/CD, Observability & Monitoring, Security, Testing, Messaging & Streaming, AI/ML)
- Methodologies & Practices
- Projects (all projects with full bullet point details)
- Volunteer Experience (all experience with full bullet point details)
- Role Types Targeted (list all roles from resume files)
- Key Themes & Strengths (top recurring themes with explanations)
- Preferred Industries / Domains
- Job Matching Notes (keywords by category, seniority signal, soft skill differentiators, geographic/logistics notes)
- Metrics & Quantified Achievements (consolidated table with averaged numbers)
    [Number each result. Include quantitative metrics wherever possible.]
    1. [Result with numbers — e.g., "Achieved 5,000x speedup over brute-force screening"]
    2. [Result — e.g., "Screened 8,500 variants, identified 7 top candidates"]
    3. [...]

## Novelty Claims
[What's genuinely new — be precise, avoid overclaiming]
- [e.g., "First application of framework X to system Y"]
- [e.g., "New method combining A and B — no prior work exists"]
```

Save the file. Show the user the complete extraction.

Progress: "Writing extraction for [short title]... [N] results identified, [M] bullet seeds drafted"

---

## Phase 4: Update Inventory

Read and update `knowledge_base/extractions/_INVENTORY_INDUSTRY.md`.

Append rows to the inventory table listing all the files (resumes) extracted:

```
| [filename] | [short title] | [date extracted] |
```

Present the updated inventory entry to the user.

---

## Phase 5: Next Steps

After extraction is complete, present:

1. **Extraction summary:**
2. **Provenance flags:** Any items that need special handling
3. **Suggested next action:**
    - If all resume folder done: "Run `/setup-industry-kb` to synthesize extractions into experience files and bundles"

### >>>>>> MANDATORY STOP <<<<<<
Present extraction summary. Wait for user feedback or next paper.
**You MUST wait for the user's explicit text response before continuing.**

---
