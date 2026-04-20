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

## Cross-domain Rule:
- If a concept appears in 2+ domains, create a separate "bridge" article
- Example: "AI navigation" bridges ai-research and iris-project
- These bridge articles are the most valuable — prioritize them

## Incremental Ingestion (tracking already-processed files):
- After ingesting a file from raw/, append its filename and a timestamp to raw/.ingested
- Format: `filename | YYYY-MM-DD HH:MM`
- When asked to "update" or "ingest new files", read raw/.ingested first, then skip any file already listed
- Always re-process a file if the user explicitly names it, regardless of .ingested
- raw/.ingested is gitignored and stays local — it is purely a local tracking log
- If raw/.ingested doesn't exist yet, create it before starting ingestion

## Linting (run periodically):
- Find orphaned articles with no backlinks
- Find rediscovery candidates
- Find temporal conflicts
- Report heat scores — highest heat = most active thinking areas
