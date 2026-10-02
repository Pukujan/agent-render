# agent-render — Project Contract

<!-- continuity:project {"id":"agent-render","protocol_version":"0.1.0-draft","schema":"project-continuity.project.v1","title":"agent-render"} -->

## Main goal

Give coding agents one place to render visual proposals — Mermaid diagrams and
architecture pages — as viewable output, so an owner can see a proposed system
rather than reconstruct it from prose.

## What this is

A rendering surface. An agent writes a proposal folder; the repository renders
it into a page. Nothing here runs a service, stores user data, or holds state.

## Product shape

- `renders/<proposal-id>/` holds one proposal: `proposal.md`, `diagram.mmd`,
  optional `notes.md`.
- Mermaid is the source of truth for diagrams; HTML is allowed for pages that a
  diagram cannot express.
- A build step renders diagrams to SVG and assembles an index of every render.
- Output is static. It can be published to any static host, including GitHub
  Pages.

## Owner authority and delivery discipline

The human owner controls scope. Agents implement the requested workflow; they do
not add unsolicited protection or architecture.

- Reuse existing OSS before writing custom code. Mermaid already renders
  diagrams; a static build already publishes pages. Do not rebuild either.
- Delivery speed is a design constraint. No optional hardening, exhaustive
  validation, or speculative future-proofing as a blocker.
- A render is evidence for a discussion. It is never a merge gate, a review
  requirement, or an acceptance criterion.

## Canonical progression

GitHub issues own scope, acceptance, and lifecycle. Merged default-branch
history owns accepted code and documents. This file is a projection of those.

## Non-goals

- a hosted service, API, or database;
- a viewer application, accounts, or permissions;
- live data, metrics, or dashboards;
- diagram authoring UI (agents author text; humans read pages);
- any format beyond Mermaid and static HTML.

## Scope control

- New dependencies require a measured need captured in an issue.
- A consuming repository may pin this one; it may not fork its own renderer.
- If a proposal needs a diagram type Mermaid cannot draw, that is an issue, not
  a silent switch to a heavier toolchain.

## Verification

A render is correct when its page opens and its diagram matches its source. No
separate evidence bundle is required.
