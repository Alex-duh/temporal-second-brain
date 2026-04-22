# 🧠 Temporal Second Brain

A personal knowledge base where **Claude Code is the librarian** and **Obsidian is the interface**. Drop raw notes, papers, exports, and conversations into `raw/` — Claude reads them, maintains a structured wiki, detects cross-domain connections, flags forgotten ideas, and lets you query everything in natural language.

Built on [Andrej Karpathy's LLM Wiki pattern](https://x.com/karpathy/status/1793294333037613357), extended with temporal heat scoring, domain clustering, bridge detection, and a live Obsidian graph view.

> **Your data never leaves your machine.** `raw/` and `wiki/` are gitignored by default.

---

## How It Works

```
raw/        ← you drop files here (stays 100% local, gitignored)
 │
 └─ Claude reads, extracts, and structures
         │
         ▼
wiki/       ← Claude maintains articles with YAML frontmatter + heat tags
         │
         ├─ Obsidian graph view   ← domain clusters, heat-colored nodes
         ├─ wiki/HEATMAP.md       ← live Dataview temporal dashboard
         └─ outputs/              ← query answers, reports (gitignored)
```

| Feature | Karpathy's LLM Wiki | This Template |
|---|---|---|
| Visualization | None | Obsidian graph + Dataview heatmap |
| Temporal layer | None | Heat scores, temperature tags, rediscovery flagging |
| Cross-domain | None | Auto-detected bridge articles |
| Input types | Web articles | Notes, PDFs, chat exports, papers, anything |
| Privacy | — | 100% local; nothing personal committed to git |

---

## Prerequisites

- **[Claude Code](https://claude.ai/code)** — the CLI that runs the agent
- **[Obsidian](https://obsidian.md)** — the vault interface (free)
- **Python 3** *(optional)* — for utility scripts
- No database, no server, no extra accounts

---

## Setup

### 1. Clone and enter

```bash
git clone https://github.com/Alex-duh/temporal-second-brain.git my-second-brain
cd my-second-brain
```

### 2. Open as an Obsidian vault

Open Obsidian → **Open folder as vault** → select the cloned directory.

The `.obsidian/` config ships with the repo, so your graph view, CSS snippets, and Dataview settings are pre-configured.

### 3. Install the Dataview plugin

In Obsidian: **Settings → Community Plugins → Browse** → search `Dataview` → Install → Enable.

The plugin config (`plugins/dataview/data.json`) is already committed — Dataview will pick it up automatically.

### 4. Create your local data folders

```bash
mkdir -p raw wiki outputs
```

These are gitignored — nothing here is ever committed.

### 5. Configure your domains in `CLAUDE.md`

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

Domain names drive the folder structure, CSS color coding, and graph clustering.

### 6. Open Claude Code

```bash
claude
```

---

## First Compilation

Drop any files into `raw/` — markdown notes, exported PDFs, `.txt` logs, anything. Then tell Claude:

```
Read everything in raw/ and compile the wiki/ folder according to CLAUDE.md.
Preserve timestamps, create wiki articles by domain, add frontmatter,
link related concepts, flag ideas older than 60 days as rediscovery candidates,
and assign heat/temperature tags to each article.
```

Claude will:
- Read every file in `raw/`
- Create structured articles in `wiki/` with YAML frontmatter
- Build backlinks between related concepts
- Auto-create **bridge articles** where ideas span multiple domains
- Flag stale ideas as rediscovery candidates
- Assign `heat/hot`, `heat/warm`, `heat/cool`, `heat/cold`, or `heat/dormant` tags
- Update `raw/processed_files.json` so future runs skip already-processed files

---

## The Obsidian Graph View

Open the graph with `Cmd+G`. The pre-configured view shows:

**Heat-colored nodes** — article temperature based on recency and engagement:
- 🔴 `heat/hot` — active in the last week
- 🟠 `heat/warm` — last 30 days
- 🟢 `heat/cool` — last 90 days
- 🔵 `heat/cold` — last 180 days
- ⬜ `heat/dormant` — older, worth revisiting

**Domain cluster labels** — `showTags: true` makes each domain tag appear as a labeled hub node. Articles in the same domain connect to their hub, forming visible named clusters (`domain/ai-research`, `domain/debate`, etc.).

**Physics tuned for clustering** — low center gravity so nodes group naturally by their actual link structure rather than collapsing to one star.

> The graph settings live in `.obsidian/graph.json`. Obsidian overwrites this file when you drag the physics sliders — if colors reset, just ask Claude to `re-apply graph config`.

---

## The Temporal Heatmap

Open `wiki/HEATMAP.md` in Obsidian (requires Dataview). You'll see:

- Every wiki article sorted by temperature (hottest first)
- Domain badge, age since last update, and heat score bar
- A live **Rediscovery Watch** table showing articles flagged as dormant

---

## Querying Your Brain

After compilation, ask Claude anything in plain English:

```
What ideas haven't I revisited in over 60 days?
Summarize everything I know about [topic] and flag any contradictions.
What are my hottest thinking areas right now?
Find all connections between my work notes and my research notes.
Write a weekly review of my most-referenced ideas.
```

Save answers to `outputs/`:

```
Save that to outputs/weekly-review-2026-04-22.md
```

---

## Adding New Notes Over Time

Drop new files into `raw/` and tell Claude:

```
Ingest new files from raw/ and update the wiki.
```

Claude reads `raw/processed_files.json` first and skips anything already ingested — only new files are processed. Existing articles are updated, not duplicated.

---

## Wiki Linting

Periodically ask Claude to audit:

```
Lint the wiki: find orphaned articles, rediscovery candidates, temporal conflicts, and report heat scores.
```

---

## File Explorer Color Coding

The `.obsidian/snippets/domain-colors.css` snippet colors the sidebar file tree by domain folder — each domain gets a distinct hue so you can navigate at a glance without opening files. Snippets are pre-enabled in `appearance.json`.

---

## Privacy & Data Safety

| Path | Tracked by git? | Leaves your machine? |
|------|----------------|----------------------|
| `raw/` | ❌ gitignored | ❌ never |
| `wiki/` | ❌ gitignored | ❌ never |
| `outputs/` | ❌ gitignored | ❌ never |
| `raw/processed_files.json` | ❌ gitignored | ❌ never |
| `CLAUDE.md` | ✅ template only | Only your domain names |
| `.obsidian/` | ✅ config only | No personal data |
| `README.md` | ✅ | This file |

Your notes are processed locally by Claude Code within your session context window. Verify with `cat .gitignore` any time.

---

## Folder Reference

```
.
├── CLAUDE.md                        ← agent instructions (edit this)
├── README.md                        ← this file
├── .gitignore
├── .claude/
│   └── settings.local.json          ← Claude Code permissions template
├── .obsidian/                       ← Obsidian vault config (pre-configured)
│   ├── graph.json                   ← heat colors + cluster physics
│   ├── appearance.json              ← CSS snippets enabled
│   ├── community-plugins.json       ← Dataview listed
│   ├── snippets/
│   │   ├── domain-colors.css        ← file explorer domain colors
│   │   └── dataview-heatmap.css     ← heatmap table styles
│   └── plugins/
│       └── dataview/
│           └── data.json            ← Dataview config (JS + HTML enabled)
├── raw/                             ← 🔒 your source files (gitignored)
├── wiki/                            ← 🔒 generated knowledge base (gitignored)
│   ├── INDEX.md
│   ├── HEATMAP.md                   ← live Dataview dashboard
│   ├── [your-domain]/
│   │   └── article.md
│   └── bridges/
│       └── cross-domain-article.md
└── outputs/                         ← 🔒 your query outputs (gitignored)
```

---

## Extending the System

- **Automated ingestion** — `claude --print "ingest new raw/ files"` on a cron/watchdog
- **Voice notes** — transcribe via Whisper, drop `.txt` into `raw/`
- **Multimodal** — screenshots, whiteboard photos (Claude Code reads images)
- **Anki export** — ask Claude to generate `outputs/flashcards.csv` from wiki articles
- **Mobile shortcut** — iOS Shortcut to append a voice memo to `raw/inbox.md`
- **Domain presets** — swap `CLAUDE.md` for a preset tuned to students, researchers, founders

---

## Stack

- **Agent:** [Claude Code](https://claude.ai/code) (Anthropic)
- **Schema:** `CLAUDE.md` instruction file
- **Storage:** Local markdown with YAML frontmatter + heat tags
- **Interface:** [Obsidian](https://obsidian.md) — graph view, Dataview, CSS snippets
- **Visualization:** Obsidian graph view (heat-colored, domain-clustered) + Dataview dashboard

---

## Contributing

Issues and PRs welcome — especially:
- Ingestion pipeline examples (Notion, Readwise, Kindle, Bear, etc.)
- `CLAUDE.md` preset templates for specific use cases (student, researcher, founder)
- Graph config improvements for better clustering
- Prompt improvements for bridge detection and temporal flagging

---

## License

MIT. Build your own brain.
