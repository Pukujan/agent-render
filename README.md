# agent-render

A render surface for coding agents. Agents produce architecture proposals as
Mermaid diagrams and HTML pages; this repository turns them into pages a human
can open and look at, instead of scrolling terminal text.

## Why this exists

Coding agents propose systems in prose. Prose hides the thing that actually
matters in an architecture decision: where the boundaries are, what talks to
what, which hop an option adds or removes. A diagram shows it in one glance.

This repo is where an agent drops a visual proposal so the owner can see it.

## What a render is

One folder per proposal under `renders/<proposal-id>/`:

```
renders/
  octo-supabase-pivot/
    proposal.md        # prose: the claim, the options, the recommendation
    diagram.mmd        # Mermaid source (flowchart, sequence, ER, state)
    notes.md           # optional: constraints, open questions
```

`proposal.md` carries the argument. `diagram.mmd` carries the picture. Both are
plain text and both are diffable, so a proposal can be reviewed like code.

## How an agent uses it

1. Write the proposal into a new `renders/<proposal-id>/` folder.
2. Commit and push to a branch, open a PR.
3. The render workflow builds the Mermaid into SVG and publishes the page.
4. Link the rendered page in the issue it belongs to.

The render is evidence for a discussion, not a substitute for one. It never
becomes a gate.

## Rendering

Mermaid is the source of truth. The build step renders `diagram.mmd` to SVG and
assembles a per-proposal page; an index lists every render. Anything that can
read Mermaid (GitHub's own `.md` preview, the VS Code extension, mermaid.live)
can display the source without this repo's build step, so a render is useful
even before it is published.

## Scope

- In: Mermaid diagrams, static HTML architecture pages, an index, a build step.
- Out: a hosted service, a database, a viewer application, live data, auth.

This is a rendering surface, not a product. Keep it small.

## Contract

`PROJECT.md` holds the goal, scope control, and non-goals. `AGENTS.md` in a
consuming repository governs how its agents use this one.
