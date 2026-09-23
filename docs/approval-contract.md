# Approval Contract

The first and second Wait nodes resume through n8n-generated webhook URLs.

## Accepted decision values

```text
approve
revise
reject
```

Decision parsing is case-insensitive.

## Request body

```json
{
  "decision": "approve",
  "reviewer": "reviewer-name",
  "feedback": "Optional review note"
}
```

## Approve

The current proposal may proceed to the executor.

## Reject

The proposal is terminated and no execution takes place.

## Revise

The reviewer feedback is passed to the LLM along with the previous proposal.

The resulting revised proposal goes through a **new approval gate**.

## Invalid decision

Anything outside the accepted values is normalized to `reject` in this portfolio workflow.

A production implementation could instead keep waiting and return a validation error to the reviewer UI.

## Audit fields

The final audit record includes:

- original request
- proposed action
- risk level
- first decision
- first reviewer
- first feedback
- revised proposal when applicable
- second decision when applicable
- execution status
- timestamps
