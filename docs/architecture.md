# Architecture

## Goal

The workflow demonstrates one principle:

> An AI-generated action is a proposal, not authorization.

## Control flow

1. A business request enters through a webhook.
2. The LLM converts the request into a structured action proposal.
3. Deterministic code classifies the action's risk.
4. The workflow builds an approval packet.
5. n8n pauses on a Wait node.
6. A human chooses **approve**, **revise**, or **reject**.
7. Approval proceeds to an allow-listed demo executor.
8. Rejection terminates safely.
9. Revision sends explicit reviewer feedback back to the model.
10. The revised proposal passes through a second Wait node.
11. The final state is converted into an audit record.

## Boundary between AI and execution

The model may propose:

- action type
- target
- draft content
- reasoning
- assumptions

The model may **not** authorize execution.

The workflow's deterministic layer controls:

- risk level
- whether approval is required
- accepted decision values
- routing
- execution status
- audit fields

## Why two approval gates?

A common HITL mistake is:

`reviewer asks for revision → AI revises → system executes`

That treats "revise" as indirect approval.

This project instead uses:

`revise → new proposal → new approval gate`

The human always sees the final version before execution.

## Executor design

The included executor is simulated so the repository is safe to import.

A production executor should use allow-listed integrations or sub-workflows. It should never execute arbitrary URLs, SQL, shell commands, or tool instructions produced directly by the LLM.
