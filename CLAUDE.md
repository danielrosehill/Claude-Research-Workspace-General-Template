# Research Workspace — Claude Code Instructions

## What This Is

This is a **private research notebook** powered by Claude Code. It uses a folder-based pattern for iterative AI research: context goes in, prompts get run, outputs get saved, and previous outputs feed back as context for deeper investigation.

## Root orientation files (read these first)

| File | Role |
|---|---|
| `CONTEXT.md` | Always-on background: topic, framing, key domain facts |
| `MEMORY.md` | Persistent-memory policy (default store: `context/from-history/`) |
| `WORKSPACE.md` | Quick path map of the folder contract below |

These are stable orientation. The live research state lives in `outputs/individual/` and `context/from-history/`.

## Research Workflow

### Running a prompt

1. Read all files in `context/` to build background understanding
2. Read the prompt file from `prompts/run/` (initial or subsequent)
3. Conduct the research using available tools (web search, document analysis, reasoning)
4. Save the output to `outputs/individual/` with format: `YYYY-MM-DD-{slug}.md`
5. If the prompt file was in `prompts/queue/`, move it to the appropriate `prompts/run/` folder after execution

### Prompts given directly in chat

If the user supplies a research prompt directly in the chat (rather than placing a file in `prompts/queue/`), first persist it before acting on it:

1. Save the prompt verbatim to `prompts/run/initial/YYYY-MM-DD-{slug}.md` (or `prompts/run/subsequent/` if it builds on prior outputs), using today's date.
2. Then process it following the normal "Running a prompt" workflow above.

This guarantees every piece of research in the repo has a corresponding, dated prompt file on disk.

### Building on previous work

Before running any subsequent prompt, always read:
- All files in `context/from-history/` (compacted prior findings)
- All files in `context/from-human/` (operator-provided material)
- The most recent outputs in `outputs/individual/` if they're relevant

### Output formatting

- Every output in `outputs/individual/` **must begin with a provenance block** before any content:

  ```
  ---
  prompt_path: prompts/run/initial/2026-04-09-example.md
  prompt_summary: One-sentence restatement of what was asked.
  run_date: 2026-04-09
  ---
  ```

  This lets any reader (and future compaction/consolidation passes) link an output back to the exact prompt that generated it.
- Use clear markdown with headers, bullet points, and tables where appropriate
- Include a `## Sources` section at the end of every research output
- Use `## Key Findings` as the opening section (immediately after the provenance block)
- Date-stamp all outputs in the filename (`YYYY-MM-DD-{slug}.md`)

### Same-day consolidation

When several prompts are run in a single day, use `/consolidate-day` to merge same-day outputs into `outputs/aggregated/markdown/YYYY-MM-DD-daily-digest.md`. After running a prompt, if other outputs from the same day already exist, proactively remind the user that `/consolidate-day` is available — do not auto-merge without being asked.

### Compaction

When instructed to compact (or when context is getting large), summarize the current research state:
- Read all files in `outputs/individual/`
- Create a comprehensive summary in `context/from-history/` named `YYYY-MM-DD-compaction.md`
- The summary should preserve key findings, sources, open questions, and contradictions
- This becomes the foundation context for subsequent research iterations

### Aggregation

When instructed to aggregate:
- Combine relevant individual outputs into a single document in `outputs/aggregated/markdown/`
- Add a cover section with research topic, date range, and methodology summary
- Compile all sources into a unified references section
- Optionally generate PDF to `outputs/aggregated/pdf/`

## Behaviour Guidelines

- Be thorough and cite sources
- Flag uncertainty and contradictions explicitly
- Distinguish between established facts, emerging consensus, and speculation
- When web searching, try multiple queries and cross-reference
- Preserve the full reasoning chain in outputs — these are research documents, not chat responses
