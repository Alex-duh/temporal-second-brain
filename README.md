# 🧠 Temporal Second Brain

A personal knowledge base where **Claude Code is the librarian**. Drop raw notes, papers, exports, and conversations into `raw/` — Claude reads them, maintains a structured wiki, detects cross-domain connections, flags forgotten ideas, and lets you query everything in natural language.

Built on [Andrej Karpathy's LLM Wiki pattern](https://x.com/karpathy/status/1793294333037613357), extended with temporal awareness, heat scoring, bridge detection, and an interactive D3.js knowledge graph.

> **Your data never leaves your machine.** `raw/` and `wiki/` are gitignored by default.

---

## How It Works

```
raw/        ← you drop files here (stays 100% local, gitignored)
 │
 └─ Claude reads everything
         │
         ▼
wiki/       ← Claude maintains structured articles with frontmatter metadata
         │
         ▼
outputs/    ← query answers, reports, heatmap.html
```

| Feature | Karpathy's LLM Wiki | This Template |
|---|---|---|
| Visualization | None | Interactive D3.js knowledge graph |
| Temporal layer | None | Heat scores, rediscovery flagging |
| Cross-domain | None | Auto-detected bridge articles |
| Input types | Web articles | Notes, PDFs, chat exports, papers, anything |
| Privacy | — | 100% local; nothing committed to git |

---

## Prerequisites

- **[Claude Code](https://claude.ai/code)** — the CLI (this repo is built for it)
- **A modern browser** — to open `outputs/heatmap.html`
- **Python 3** *(optional)* — only if you write custom scripts
- No database, no server, no extra accounts

---

## Setup

### 1. Clone and enter

```bash
git clone https://github.com/Alex-duh/Dudu-secondbrain.git my-second-brain
cd my-second-brain
```

### 2. Create your local folders

```bash
mkdir raw wiki outputs
```

These are gitignored — nothing here is ever committed.

### 3. Configure your domains in `CLAUDE.md`

Open `CLAUDE.md` and replace the example domains with your own life areas:

```markdown
## Domains
- work          (projects, decisions, meeting notes)
- learning      (books, courses, papers, tutorials)
- project-ideas (prototypes, half-baked builds)
- personal      (goals, habits, journal fragments)
- research      (citations, lab notes, papers)
- bridges       (cross-domain connections — keep this one)
```

Be specific. `"machine-learning"` beats `"stuff"`. Domain names become the backbone of your graph.

### 4. Open Claude Code

```bash
claude
```

---

## Running Your First Compilation

Drop any files into `raw/` — markdown notes, exported PDFs, `.txt` conversation logs, screenshots, anything. Then tell Claude:

```
Read everything in raw/ and compile the wiki/ folder according to CLAUDE.md.
Preserve timestamps, create wiki articles by domain, add frontmatter,
and link related concepts with backlinks. Flag ideas older than 60 days
as rediscovery candidates. When done, summarize what you created and
what cross-domain bridges you found.
```

Claude will:
- Read every file in `raw/`
- Create structured articles in `wiki/` with frontmatter metadata
- Build backlinks between related concepts
- Auto-create **bridge articles** where ideas span multiple domains
- Flag stale ideas (>60 days, no links) as rediscovery candidates
- Report a heat ranking of your most-referenced thinking areas

A fresh compilation of ~20 source files typically takes 2–3 minutes.

---

## Querying Your Brain

After compilation, ask Claude anything in plain English:

```
What ideas haven't I revisited in over 60 days?
```
```
Summarize everything I know about [topic] and flag any contradictions.
```
```
What are my hottest thinking areas right now based on heat scores?
```
```
Find all connections between my work notes and my learning notes.
```
```
Write a weekly review of my most-referenced ideas.
```

Save answers to `outputs/` for future reference:

```
Save that to outputs/weekly-review-2024-12-18.md
```

---

## The Knowledge Graph

Generate or refresh your personal interactive heatmap at any time:

```
Regenerate outputs/heatmap.html from the current wiki.
```

Open it in any browser:

```bash
open outputs/heatmap.html   # macOS
xdg-open outputs/heatmap.html  # Linux
start outputs/heatmap.html  # Windows
```

> `outputs/heatmap.html` is gitignored — it's your personal graph and stays local.
> `demo/heatmap.html` is the public demo with fake data, tracked in the repo.

**What you'll see:**
- Every wiki article as a **node**
- Node **size** ∝ backlink count — bigger = more referenced
- Node **color** = domain
- Bridge nodes **pulse red** — your most valuable cross-domain insights
- **Dashed border** = rediscovery candidate (stale, worth revisiting)
- Click any node → metadata panel with heat score and clickable related articles
- Drag, zoom, and pan freely

The `demo/heatmap.html` shipped with this repo uses fake data so you can see the visualization immediately. After your first compilation, generate your personal one with real wiki data — it lives at `outputs/heatmap.html` (local only, never committed).

---

## Adding New Notes Over Time

You don't need to recompile everything each time. Just drop new files into `raw/` and tell Claude:

```
I added new files to raw/ — ingest them and update the wiki.
Don't touch existing articles unless something changes.
```

Claude will process only new or modified source files, update relevant articles, increment heat scores, and create new bridge articles if new cross-domain patterns emerge.

---

## Wiki Linting

Periodically ask Claude to audit the wiki:

```
Lint the wiki: find orphaned articles, rediscovery candidates,
temporal conflicts, and report heat scores.
```

This surfaces:
- **Orphans** — articles with no backlinks (possibly need connecting or pruning)
- **Rediscovery candidates** — old ideas not linked anywhere (worth revisiting)
- **Temporal conflicts** — notes that contradict each other over time
- **Heat ranking** — your most active areas of thinking right now

---

## Privacy & Data Safety

| Path | Tracked by git? | Leaves your machine? |
|------|----------------|----------------------|
| `raw/` | ❌ gitignored | ❌ never |
| `wiki/` | ❌ gitignored | ❌ never |
| `outputs/*.md` | ❌ gitignored | ❌ never |
| `CLAUDE.md` | ✅ template only | Only your domain names |
| `outputs/heatmap.html` | ✅ demo data only | Only demo data |
| `README.md` | ✅ | This file |

Your notes are processed locally by Claude Code within your active session context window. Verify with `cat .gitignore` any time.

---

## Folder Reference

```
.
├── CLAUDE.md                  ← agent instructions (edit this for your setup)
├── README.md                  ← this file
├── .gitignore                 ← keeps your personal data local
├── .claude/
│   └── settings.local.json    ← Claude Code permissions
├── raw/                       ← 🔒 your source files (gitignored)
├── wiki/                      ← 🔒 generated knowledge base (gitignored)
│   ├── INDEX.md
│   ├── [your-domain]/
│   │   └── article.md
│   └── bridges/
│       └── cross-domain-article.md
├── demo/
│   └── heatmap.html           ← 🌐 public demo visualization (fake data, tracked)
└── outputs/                   ← 🔒 your personal outputs (gitignored)
    └── heatmap.html           ← your real knowledge graph (regenerate anytime)
```

---

## Extending the System

This template is designed to grow. Roadmap ideas:

- [ ] **Automated ingestion** — `claude --print "ingest new raw/ files"` on a cron/watchdog
- [ ] **Voice notes** — transcribe via Whisper, drop `.txt` into `raw/`, Claude handles the rest
- [ ] **Multimodal** — screenshots, whiteboard photos, annotated PDFs (Claude Code reads images)
- [ ] **Anki export** — ask Claude to generate `outputs/flashcards.csv` from wiki articles
- [ ] **Temporal decay** — have Claude lower heat scores on un-referenced articles weekly
- [ ] **Apple Notes / Obsidian / Notion pipeline** — export as markdown, drop in `raw/`
- [ ] **Mobile shortcut** — iOS Shortcut to append a voice memo transcript to `raw/inbox.md`
- [ ] **CLAUDE.md presets** — domain templates for students, researchers, founders, writers

To add a feature: describe the behavior you want in `CLAUDE.md` and Claude will follow it on the next run. No code needed for most extensions.

---

## Stack

- **Agent:** [Claude Code](https://claude.ai/code) (Anthropic)
- **Schema:** `CLAUDE.md` instruction file
- **Storage:** Local markdown files with YAML frontmatter
- **Visualization:** [D3.js v7](https://d3js.org) force-directed graph
- **Editor (optional):** [Obsidian](https://obsidian.md) for browsing the compiled wiki

---

## Contributing

Issues and PRs welcome — especially:
- Ingestion pipeline examples (Notion, Readwise, Kindle, Bear, etc.)
- `CLAUDE.md` preset templates for specific use cases
- Alternative visualization layouts or themes
- Prompt improvements for better bridge detection and temporal flagging

---

## License

MIT. Build your own brain.
