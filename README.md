# Futures Expiry Workflow Agent

An agentic ops workflow for daily futures contract expiry management.

## Background

At my firm, the daily futures expiry workflow involved 5 manual steps with no 
enforced sequencing. A bug was discovered where deactivating expired contracts 
didn't cascade-clean their margin tier records — orphaned records would sync to 
the front-end system, showing traders incorrect margin data.

This agent redesigns that workflow as a dependency pipeline with validation gates.

## Live Demo

👉 https://hyzz97.github.io/futures-expiry-agent

## How It Works

| Step | Action | Gate |
|------|--------|------|
| 1 | Query expiring contracts | — |
| 2 | Validate margin tier records | ❌ Halt if orphaned records found |
| 3 | Notify trading desk | — |
| 4 | Deactivate + cascade clean | — |
| 5 | Post-deactivation validation | ❌ Halt if stale records remain |

## Key Design Principles

- **Dependency chain, not checklist** — each step gates the next
- **Cascade cleanup** — deactivation and margin tier deletion are atomic
- **Fail loudly** — validation failure halts pipeline immediately
- **Human-in-the-loop** — agent owns execution, human signs off on summary

## Try It

- **▶ Run Clean** — normal workflow, all 5 steps complete
- **⚠ Inject Bug** — simulates orphaned margin records, validation gate catches it
