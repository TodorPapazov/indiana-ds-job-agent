# Indiana DS Job Agent

Daily agent that scrapes Indianapolis metro and Indiana data science, ML, GenAI, and analytics engineering job postings, scores them against a target profile, and commits a ranked digest to this repo.

Built as an **agentic development starter project** using Grok CLI (Grok 4.7) — the goal is hands-on practice with multi-step agent workflows: search, extract, score, validate, and persist.

## Why this exists

Transitioning from Operations Data Analyst (TRIMEDX, healthcare) toward AI/ML/GenAI engineering roles. This repo turns the daily job search into a repeatable agent pipeline instead of manual browsing — and doubles as portfolio evidence of agentic AI work.

## Architecture

```
Scheduler (Grok Automations, weekdays 8 AM ET)
        |
        v
+------------------+
|  SEARCH AGENT    |  Web search: Indeed, LinkedIn, Built In,
|                  |  Wellfound, company career pages
|                  |  (Lilly, Corteva, Amgen, Cummins, Elanco,
|                  |   Ascension, Deloitte, Roche, Micro1...)
+------------------+
        |
        v
+------------------+
|  EXTRACT AGENT   |  Title, company, location, salary,
|                  |  required skills, posting URL
+------------------+
        |
        v
+------------------+
|  SCORE AGENT     |  1-10 fit vs target profile
+------------------+
        |
|  (guardrail: max 10 listings, skip duplicates)
        |
        v
+------------------+
|  PERSIST AGENT   |  Commit indiana-ds-digest-YYYY-MM-DD.md
|                  |  to main
+------------------+
```

Each stage is a discrete step with a defined input and output. That separation is the core agentic pattern — swap any stage's implementation without touching the others.

## Target profile (scoring rubric)

See **PROFILE.md** for the full scoring target. Summary weights:

| Dimension | Weight | What counts |
|---|---|---|
| Python + SQL | 20% | Production SQL, Snowflake, pipelines |
| ML frameworks | 20% | scikit-learn, PyTorch, TensorFlow |
| Cloud | 15% | AWS, Azure, GCP, Databricks, Docker/K8s |
| GenAI / RAG / LLMs | 20% | RAG, fine-tuning, LLM eval, vector DBs |
| Agentic AI | 10% | CrewAI, LangGraph, agent orchestration |
| Healthcare domain | 10% | Clinical ops, EHR/CMMS, value-based care |
| Analytics stack | 5% | Power BI, DAX, Fabric, dashboards |

Score 1-10. Roles scoring 7+ get a one-line "why it fits" in the digest.

## Output format

Daily file: `indiana-ds-digest-YYYY-MM-DD.md`

```markdown
# Indiana DS Digest — 2026-10-17

## Top 5
1. **Title** — Company (location, salary)
   - Fit: 8/10 — reason
   - URL
...
## All listings
| # | Title | Company | Location | Salary | Score | URL |
|---|---|---|---|---|---|---|
...
## Notes
- Anything unusual: new employers, salary trends, skill shifts
```

## Grok CLI workflow

```bash
# Clone
git clone https://github.com/TodorPapazov/indiana-ds-job-agent.git
cd indiana-ds-job-agent

# Run a manual digest (headless)
grok build run --headless "Search today's Indiana DS/GenAI postings,
score against PROFILE.md, write indiana-ds-digest-YYYY-MM-DD.md,
commit and push to main."

# Or interactive plan-then-execute
grok build run --plan
```

## Roadmap

- [x] Repo + daily automation scaffold
- [x] PROFILE.md with full scoring weights
- [ ] Python CLI version (`python agent.py`) using jobspy
- [ ] FastAPI wrapper + Docker container
- [ ] RAG layer: embed past digests, surface historical trends
- [ ] Slack/email notification on 8+ scores
- [ ] Deploy to a small VM or Hugging Face Space

## Stack

- **Agent runtime:** Grok CLI (Grok 4.7), headless + plan modes
- **Scheduling:** Grok Automations (weekday 8 AM ET)
- **Persistence:** GitHub main branch
- **Future:** Python, jobspy, FastAPI, Docker, sentence-transformers

## License

MIT
