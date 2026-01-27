# CC Workshop Series

A 3-part workshop series (90 minutes each) for GTM teams, teaching practical Claude Code workflows.

| # | Workshop | Speaker | Focus |
|---|----------|---------|-------|
| 1 | Context OS Starter Kit | Jacob | Build a file-based "company brain" from working docs and transcripts |
| 2 | Reusable GTM Skills | Joe | Create reusable skills that turn websites into structured GTM outputs |
| 3 | ICP → TAM Lists + Enrichment | Eric | Mine ICP patterns, build prospect lists, apply channel strategy |

---

## Repo Structure

```
cc-workshops/
├── README.md                      ← you are here
├── workshop-context.md            ← master outline (full series overview)
├── workshops/
│   ├── 1-context-os-jacob.md      ← Jacob's workshop details
│   ├── 2-gtm-skills-joe.md       ← Joe's workshop details
│   └── 3-icp-tam-eric.md         ← Eric's workshop details
├── transcripts/                   ← source transcripts
│   ├── jacob-transcript.txt
│   ├── joe-transcript.txt
│   └── eric-transcript.txt
└── site/                          ← landing page (coming soon)
```

---

## How to Contribute

Each workshop speaker owns their file in `workshops/`. Here's how to make edits:

### 1. Clone the repo

```bash
git clone https://github.com/winterje90/cc-workshops.git
cd cc-workshops
```

### 2. Create your branch

```bash
# Jacob:
git checkout -b jacob/workshop-1

# Joe:
git checkout -b joe/workshop-2

# Eric:
git checkout -b eric/workshop-3
```

### 3. Edit your workshop file

Open the repo in **VS Code** or **Cursor** with the **Claude Code extension**. Your file is in `workshops/`:

- Jacob → `workshops/1-context-os-jacob.md`
- Joe → `workshops/2-gtm-skills-joe.md`
- Eric → `workshops/3-icp-tam-eric.md`

You can use Claude Code to help iterate (e.g., "refine the hands-on activities", "add more detail to the 25-40 min section", etc.).

You can also reference your transcript in `transcripts/` for source material.

### 4. Commit and push

```bash
git add .
git commit -m "update workshop content"
git push -u origin your-branch-name
```

### 5. Open a Pull Request

Go to the repo on GitHub and open a PR. James will review and merge.

---

## Questions?

Reach out to James.
