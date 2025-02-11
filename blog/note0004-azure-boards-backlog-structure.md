---
slug: azure-boards-backlog-structure
title: 'Note 4: Azure Boards Backlog Structure'
authors: ivanignatiev
tags: []
date: 2025-02-11T09:30
---

## Small 1 Team Projects

Use default or customized SCRUM process. Optionally customize status (e.g., "Proposed” → "To Do,” "Committed” → "In Progress,” "Done” remains "Done”) and add custom fields to explicit product components.

Below is a backlog structure, which you can easily visualize in Delivery Plans, to maintain flexibility for reporting:

1. Product track:

- **Epic**: formalize an objective in SMART way, description with "Why?"; when versioning lifecycle is well defined, the Epic can be as simple as "Product v10"
- **Feature**: describe concrete deliverable which can require multiple sprints to deliver, which could have any external dependencies, or which could require multiple team members to collaborate ("AI App for Model X" can take effort from API + Data & AI + UI)
- **PBI**: 1 sprint concrete deliverable with 1 concrete responsible person ("1 REST API Endpoint to call model prediction" - API deliverable with Lead assigned, "Prediction visualizations X, Y, Z" - UI deliverable with Lead assigned, etc.)
- **Task** (Optional): Tasks help explicit dependencies within PBI which requires multiple persons to collaborate 

2. Continuous tracks (i.e. ideas, quality or tech debt):

- Epic as a bucket
- PBI for ideas and tech debt
- Bugs for quality issues

Personally, I like [Migration](https://bulletjournal.com/blogs/faq/migration) idea of bullet journal for refinement. If objective is changed, recreate a structure from Epic to Task. If more fine-grained tracking tracking is required, the items can be moved and reformulated to keep the same work item ID and change history.

Avoid process over-engineering and adopt the level of details to team's context. Here is some ideas how to do it: [Principles over Process - svpg](https://www.svpg.com/principles-over-process/)

## References

Best courses:

- [Alan Slater - Build & Manage your Requirements Backlog in Azure Devops](https://www.udemy.com/course/azure-devops-build-and-manage-your-requirements-backlog/?couponCode=JUST4U02223)
- [Alan Slater - Agile Requirements Mastery: Essential skills in just 2 hours](https://www.udemy.com/course/write-great-agile-requirements-in-just-118-minutes/?couponCode=JUST4U02223)

AI to break up tasks in smaller peaces:

- [goblin tools](https://goblin.tools/)