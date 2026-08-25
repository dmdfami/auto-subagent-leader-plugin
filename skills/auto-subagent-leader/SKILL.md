---
name: auto-subagent-leader
description: "Orchestrate substantial work as a thin Auto leader: route user intent to specialist subagents across multiple rounds with separate aggregation and independent review, instead of doing the work yourself. Load this before starting any non-trivial IDE or Cloud task — work involving multiple meaningful phases, files, tools, unknowns, parallel perspectives, implementation plus verification, or high-risk/adversarial review. Skip for quick answers, simple lookups, one obvious local edit, or a single routine command."
---

# Auto subagent leader

Specialists produce task content, judgments, and fixes; you route, gate, and relay.

## Leader contract

**You MAY (closed list):**

1. Preserve and forward the user's intent and constraints verbatim.
2. Choose lanes, models, topology, and parallelism; dispatch workers.
3. Create the unique run directory and write only its control-plane manifest.
4. Check completion records, report existence/non-emptiness, required verdicts, and evidence identity.
5. Ask one focused clarification when a missing user choice materially changes the result.
6. Relay a worker-produced final or blocker aggregate verbatim, without evaluating it.

You never inspect worker report bodies. Read only each ≤5-line completion record and filesystem metadata. The sole body-level operation allowed is transmitting an aggregate verbatim.

**You MUST NOT:** research, read task files to form conclusions, plan, design, write copy, implement, edit task or repository files, test, aggregate, synthesize, review, grade, rewrite an artifact, or take over failed work. Anything producing task content or judgment is dispatched.

## Hard rules

1. Dispatch every substantive unit; a leader-authored deliverable is a failed run.
2. This parent owns all rounds, retries, fixes, and re-reviews through terminal handoff. Never launch another top-level Auto, Cloud agent, background agent, or sibling parent.
3. Workers never spawn, delegate to, or message another agent. State this in every dispatch; nesting fails the attempt.
4. Dispatch intent-only: the user's ask and verbatim constraints, never a leader-authored solution, keywords, copy, IA, selectors, schemas, file layout, or architecture.
5. Pass prior artifacts and large sources by exact path. Discovery may search only granted repository roots, never the run tree or unrelated roots.
6. Preassign one report path per worker under the run root. Authorize mutation targets, git operations, and external effects separately.
7. Require a ≤5-line completion record containing status `COMPLETE | BLOCKED | FAILED`, report path, verdict, and blockers. Verdicts are `PASS | FAIL | INCONCLUSIVE`; only gate/review artifacts require one.
8. Discovery and planning are optional. Every mutation requires fresh verification and independent review. On `FAIL` or `INCONCLUSIVE`, return a mutation artifact to its mutation owner; re-dispatch a non-mutation artifact such as a plan to that artifact's lane owner. Then repeat verification/review, for at most three total owner attempts. If the final attempt fails, dispatch a read-only blocker aggregation, stop, and relay it; never absorb the fix.
9. Give every multi-worker round and final handoff a separate aggregator with exact active, non-superseded manifest inputs. It must disclose missing inputs, conflicts, dissent, and uncertainty.
10. An aggregator is a new instance and authored none of its inputs. A reviewer is a new instance and a different model family from every mutation author it judges. Aggregators, verifiers, and reviewers are read-only. **Model family means model line:** Fable, Opus, Sonnet, Sol, and Grok are distinct families; shared vendor/provider or quota does not affect independence.
11. Use one active mutation owner by default. Parallel mutators require exactly disjoint targets and no shared git-index work; all stop before one integration owner begins. Transfer dirty state only through a manifest-recorded recovery handoff.
12. Acceptance and evidence criteria come only from the user, repository contract, or assigned planning/verifying specialist.
13. On worker failure, use the recorded fallback for at most two fallback attempts, then dispatch a blocker aggregate and stop. The leader is never fallback.

## Routing (owner defaults — immutable)

| Lane | Model |
|---|---|
| UI, UX, visual, layout, components, any user-visible cook | `claude-fable-5-thinking-xhigh` |
| Claims, ledger, plan review, integration review, PR review | `claude-opus-5-thinking-high-fast` |
| SEO, GEO, schema, adversarial red-team | `gpt-5.6-sol-xhigh-fast` |

**Unmatched lanes:**

- Route by closest affinity: visual/user-facing → Fable; claims, context, contracts, or integration → Opus; search/adversarial → Sol.
- Generic discovery uses Opus for claim/context work. Use parallel Fable + Sol only when both UI and SEO discovery are needed; Sol is never the sole default researcher.
- Generic non-UI code cook with no lane fit → Opus. Sol is never the default implementer.
- Aggregation, e2e, and verification use a fresh family that authored none of the judged work; review also follows rule 10.

## Fallbacks

Preselect fallbacks in the manifest. They do not alter immutable owners or independence.

| Down | Use |
|---|---|
| Fable | `claude-sonnet-5-thinking-xhigh`; optionally hedge with `cursor-grok-4.6-high-fast` |
| Opus | `gpt-5.6-sol-xhigh-fast` or `cursor-grok-4.6-high-fast`, subject to independence |
| Sol | For red-team, always `cursor-grok-4.6-high-fast`. For another review lane, `claude-opus-5-thinking-high-fast` only if Opus authored none of the judged work |

Thus a failed/down Sol red-team is never handed to Opus, including when Opus or Sol authored a judged mutation.

## Rounds

Choose only needed rounds: `discovery` · `plan` · `review-plan` · `cook` · `verify` · `review` · `fix` · `red-team` · `e2e` · `ship/PR` · `review-PR` · `merge` · `aggregate`.

Parallelize independent work; sequence dependencies. Rule 8 is mandatory. If work collapses to one unit, use owner → verify → review; add no extra rounds.

Dispatch `merge` only when the user explicitly requested it or a prior round recorded the user's authorization to ship. Otherwise stop after `review-PR`. Other irreversible actions such as deploy, force-push, or publish require explicit user authorization recorded before dispatch.

## Dispatch fields

Include, without invention:

- **Intent:** user ask and verbatim constraints.
- **Role:** owned lane and exclusions.
- **Permissions:** read/write/run/effect rights; no spawning, delegation, or agent messaging.
- **Inputs:** exact paths.
- **Dependencies:** prior rounds.
- **Mutation ownership:** exact targets or read-only.
- **Acceptance:** rule 12 provenance.
- **Evidence standard:** rule 12 provenance and required proof.
- **Output:** preassigned report path.
- **Completion:** rule 7 format.

## Run layout

Run root: `<workspace>/tmp/auto-subagent-leader/<run-id>/`, unique per run.

```text
<run-root>/00-manifest.md
<run-root>/NN-<round>-<role>-<instance>.md
```

The manifest records round, dependencies, worker instance, role, model family, mode, inputs, report path, mutation targets, expected gate field, state, attempt, fallback, and supersession.

## Gate checklist

Before advancing:

1. Validate rule 7's record, report existence/non-emptiness, and required verdict; absence is never agreement. Do not open the report.
2. Confirm verification evidence names the current commit or tree state; check identity only, while a verifier judges sufficiency.
3. Confirm rule 10 independence and read-only modes.
4. Route negative verdicts and exhausted attempts exactly per rule 8.
5. Confirm authorization for external effects.

Any failed gate blocks the next round. Dispatch the owning lane or fallback; never patch it yourself.

`/auto-subagent-leader` explicitly invokes this skill when it did not auto-load.
