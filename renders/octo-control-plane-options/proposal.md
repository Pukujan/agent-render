# Control-plane options — proposal

**Date:** 2026-10-02
**Status:** proposal for discussion, no decision taken
**Issue:** octo-database #71
**Related:** agent-render #1

> Web search was unavailable when this was written. Third-party product details
> are marked *(verify)* and need checking against current docs before a decision.

## The claim

Octo hand-built a control plane that a mature product already provides. The
schema proves the intent: it was written for Supabase Auth, then the runtime
quietly replaced it with custom code. The result is a system that is missing
features a mature stack gives for free, while carrying maintenance it did not
need to own.

## The finding that matters

**RLS is declared but not enforced.** All 70 policy statements use `ENABLE ROW
LEVEL SECURITY`, never `FORCE`. The server connects as the `postgres` superuser,
which bypasses RLS. There is no `SET ROLE` and no `auth.uid()` binding in `src/`.

So the security model exists on paper and application code is what actually
authorizes. Moving to the real Supabase stack would **activate the RLS work
already committed**, not discard it.

## The options

| | Self-hosted | Console per workspace | Maintenance | Keeps Octo's custom work |
|---|---|---|---|---|
| **Today** | yes | n/a (hand-built) | high | n/a |
| **A — Supabase Cloud** | no | yes | none | archive, keys, shares |
| **B — one Supabase, RLS tenancy** | yes | no (shared Studio) | medium | archive, keys, shares |
| **C — Appwrite / Nhost / Hasura** | yes | partial *(verify)* | medium | little carries over |
| **D — CloudNativePG + pgAdmin** | yes | no (one pgAdmin) | high | archive, keys, shares |

See `diagram.mmd` for the stacks side by side.

## What no path removes

The archive lifecycle, scoped agent keys, the job scheduler, gallery, and share
links have no equivalent in any mature stack. They stay custom under every
option. See `survivors.mmd`. This is the part that was always genuinely Octo.

## The projection question

Supabase covers Postgres only. pgvector lives inside Postgres and needs no
separate console. Neo4j and DuckDB do not fit inside Supabase, and unifying them
under one custom dashboard is the same over-build trap.

**Octo should be a launcher, not a unified console.** Each projection keeps its
native console; Octo links to it. The dashboard already does this with its
external-console section.

## Recommendation

Not a settled recommendation — this is for the owner. The reasoning:

- If "self-hosted" is a preference rather than a requirement, **Path A** is by
  far the least work and the most mature.
- If self-hosting is required, **Path B** finishes the migration the schema
  already started, with the least new infrastructure.
- **Path D** gives the most control and the most operational burden, and
  Kubernetes is currently an explicit Octo non-goal.

## Open decision

The mapping in §6.1 of the research checkpoint: **one project per workspace**, or
**one project with RLS tenancy**. Nothing else can be designed until this is
answered.
