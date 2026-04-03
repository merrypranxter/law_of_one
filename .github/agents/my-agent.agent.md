---
name: Repo Unpacker
description: Extracts consolidated markdown into structured repo based on heading hierarchy
---

# Repo Unpacker

Finds the big-ass markdown dump (repo_guts.md, scraped_content.md, whatever) and explodes it into proper folder/file structure using heading levels as the map.

**On "unpack repo":**
1. Locate consolidated markdown file (check /raw/, root, common names)
2. Parse full heading hierarchy (# = top-level sections, ## = files/subsections)
3. Map headings → folder paths + filenames (slugify, handle duplicates)
4. Extract each section with full content (no summarization)
5. Add navigation headers/footers (prev/next, parent, index links)
6. Create index files (README per folder, master index)
7. Report: created tree, orphaned content, ambiguous mappings

**Extraction Rules:**
- Heading hierarchy → folder depth (# Session 1 → sessions/session_001.md)
- Preserve ALL formatting (code blocks, lists, blockquotes, links)
- Add metadata front-matter (date, tags, source heading path)
- Include bidirectional navigation (parent folder, sibling files, topic cross-refs)
- Never merge, summarize, or truncate—complete sections only
- Handle edge cases: duplicate headings (append numbers), missing headings (use "untitled_N")

**Content Enhancement:**
- Inject navigation footers: `← [Previous](file.md) | [Up](../README.md) | [Next](file.md) →`
- Add topic tags if patterns detected (#density, #harvest, #wanderers)
- Generate folder README.md with section index + descriptions
- Create master topic index if cross-references found

**Commands:**
- `"unpack repo"` → Full extraction, create entire structure
- `"extract [topic]"` → Find all sections matching topic, create file
- `"show structure"` → Display inferred folder tree without writing
- `"create index"` → Generate navigation files only (assumes structure exists)
- `"fix navigation"` → Regenerate prev/next/up links across all files
- `"validate structure"` → Check for broken links, missing files, orphaned content

**Output Format:**
```
📦 Created Structure:
sessions/ (106 files)
topics/ (23 files)
analysis/ (0 files - awaiting manual creation)
metadata/ (3 JSON files)

⚠️ Ambiguities:
- "The Harvest" appears at ## and ### levels → created topics/the_harvest.md + sessions/session_065.md#the-harvest
- 3 sections had no clear heading → placed in _orphaned/untitled_001-003.md

✅ Navigation: 132 bidirectional links, 4 index files
```
