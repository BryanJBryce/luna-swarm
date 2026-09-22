---
name: luna-swarm
description: Coordinate bounded parallel work with GPT-6 Luna agents under a primary coordinator. Use when the user explicitly invokes $luna-swarm or asks for a Luna swarm, broad parallel exploration, independent reviews, test or log analysis, research fan-out, or implementation split across independent areas. Do not trigger for ordinary tasks that do not request delegation.
---

# Luna Swarm

Coordinate parallel work with GPT-6 Luna subagents. The primary agent owns scope,
decomposition, integration, decisions, and final verification.

## Preserve authority

- Delegation covers only work already authorized by the user.
- Preserve existing approval, sandbox, network, write, and external-action boundaries.
- Do not broaden the task to keep workers busy.
- Apply newer user direction to work that is still in progress.

## Choose the swarm shape

- Aim for up to ten workers when there are that many useful independent slices.
- Use fewer workers when extra assignments would duplicate effort or add coordination cost.
- Prefer parallel exploration, audits, test and log analysis, research, and other read-heavy work.
- Parallelize implementation only when each worker has a distinct write area.
- Keep cross-cutting design, shared-file edits, integration, and final verification with the primary agent.
- Treat workers as leaf agents by default. Allow nested delegation only when the scope and runtime budget clearly support it.

## Select the worker model

- Select `gpt-6-luna` explicitly for each worker when the spawn interface supports model selection.
- Use `medium` reasoning by default, unless the user asks for a different effort.
- Start workers with fresh context when the runtime supports it.
- If explicit model selection is unavailable, rely on a default only when the active Codex configuration confirms that spawned agents use `gpt-6-luna`.
- Treat an accepted spawn selection or persisted session metadata as evidence of the worker model. Do not rely on a worker's textual self-identification.
- Do not silently substitute another model. Explain the limitation if Luna cannot be selected or verified.
- Do not patch or replace the runtime's model catalog. If the runtime cannot start Luna agents, report the blocker.

## Decompose before spawning

Inspect enough context to define independent assignments. For each worker, specify:

- One objective and clear non-goals.
- Relevant paths, symbols, inputs, and constraints.
- Read-only status or exclusive write ownership.
- Evidence, checks, or validation required.
- The project's verification command and who owns the integrated full check.
- A concise result format with paths and specific findings.

Give every worker enough context to act independently. Do not send unrelated conversation, broad logs, or unnecessary repository contents.

Use this prompt shape:

```text
Complete this bounded task: <objective>.
Scope: <paths, symbols, inputs>.
Do not: <non-goals>.
Write ownership: <read-only or exclusive paths>.
Verify with: <project runner and checks; integrated full-check owner>.
Return: <concise evidence and result format>.
Do not expand scope or spawn subagents.
```

## Execute and coordinate

1. Map the assignments before launching workers.
2. Launch independent work concurrently, within the runtime's limits.
3. Use bounded waves if there are more assignments than available worker slots.
4. Do not redo delegated work while a worker is active.
5. Steer a worker when it is blocked, drifting, or missing required evidence.
6. Retry a failed slice once with a corrected, self-contained prompt when useful.
7. Resolve disagreements from concrete evidence; use another worker as a tie-breaker only when needed.
8. Integrate results and run the final end-to-end verification in the primary agent.

## Protect shared work

- Assume workers can see concurrent edits and may share access to files and services.
- Give concurrent writers distinct paths and require them to preserve unrelated changes.
- Avoid overlapping work on mutable shared resources; separate checkouts or databases do not necessarily isolate service capacity.
- Follow the repository's documented test and verification workflow. Keep one owner for the integrated full check.
- Stop or reassign a worker that crosses its write boundary.
- Do not ask workers to commit, push, deploy, publish, or perform external writes unless the user separately authorized that action.

## Synthesize the result

Return one integrated report that states:

- The outcome and important evidence, including relevant paths.
- Verification performed by the primary agent.
- Material disagreements, failed work, or remaining uncertainty.
- The number of Luna workers used if it differs from the requested number.

Do not claim work from workers that failed to start or finish.
