# Second Brain — Alex Du's Personal Knowledge Base

## Folder Structure
- raw/ — all source files, NEVER modify these
- wiki/ — you maintain this entirely, I never edit it
- outputs/ — write query answers here as .md files

## When ingesting files in raw/:
1. Read fully, identify content type (research, conversation, note, paper)
2. Extract key concepts, ideas, arguments, dates
3. Check if concepts already exist in wiki/ — update rather than duplicate
4. Create or update wiki articles with rich backlinks
5. Preserve original timestamps as frontmatter metadata
6. Flag any idea that appears in multiple sources as "cross-domain insight"

## Every wiki article MUST have this frontmatter:
---
title:
domain:
source:
date_created:
last_updated:
related: []
rediscovery: false
heat: 0  # increment each time this article is referenced or updated
---

## Domains (be specific when categorizing):
- ai-research (IRIS, agents, LLMs, computer vision)
- iris-project (iris paper, iris poster, hardware, trials, IRB, navigation, competitions such as Barron Prize)
- debate (PF cases, frameworks, evidence, tournament notes, debate notes, critical literature, lacan, psychoanalysis)
- cs-concepts (Java, recursion, algorithms, AP CSA)
- apush (periods, themes, exam prep)
- physics (ap preparation, Calculus based physics related concepts)
- personal-ideas (shower thoughts, future projects, half-baked concepts)
- college-apps (essays, schools, strategy, counselor notes)
- research (papers, notes, Pitt Intelligent System Lab)

## Temporal Rules:
- Preserve creation dates from source files whenever possible
- Any idea older than 60 days not referenced elsewhere → set rediscovery: true
- When two notes from different dates contradict → flag as "temporal conflict"
- Increment heat score each time an article gets updated or linked to
- After updating heat or last_updated, recalculate and update the `tags:` field:
  - score = max(0, days_since_last_updated - heat * 5)
  - score ≤ 7   → tags: ["heat/hot"]      (red in graph)
  - score ≤ 30  → tags: ["heat/warm"]     (orange in graph)
  - score ≤ 90  → tags: ["heat/cool"]     (green in graph)
  - score ≤ 180 → tags: ["heat/cold"]     (blue in graph)
  - score > 180 → tags: ["heat/dormant"]  (gray in graph)

## Cross-domain Rule:
- If a concept appears in 2+ domains, create a separate "bridge" article
- Example: "AI navigation" bridges ai-research and iris-project
- These bridge articles are the most valuable — prioritize them

## Incremental Ingestion (tracking already-processed files):
- Manifest lives at `raw/processed_files.json`
- Before ingesting, read the manifest and skip any filename already listed under `"files"`
- After ingesting, add or update the entry:
  ```json
  "filename.md": {
    "ingested_at": "YYYY-MM-DDTHH:MM:SS",
    "wiki_articles": ["wiki/domain/article.md"],
    "domains": ["domain-name"],
    "content_type": "note|paper|conversation|research|document"
  }
  ```
- Always re-process a file if the user explicitly names it — update the entry with a fresh `ingested_at`
- If `processed_files.json` doesn't exist, create it with `{"version": 1, "last_updated": "...", "files": {}}`
- Update `last_updated` at the top-level each time the manifest is written
- This file is gitignored and stays local

## Linting (run periodically):
- Find orphaned articles with no backlinks
- Find rediscovery candidates
- Find temporal conflicts
- Report heat scores — highest heat = most active thinking areas
