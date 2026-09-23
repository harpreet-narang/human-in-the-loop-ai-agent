# Deterministic Risk Policy

The AI proposes an `action_type`. JavaScript maps that type to a risk level.

| Action type | Risk | Rationale |
|---|---|---|
| `draft_only` | low | No external side effect |
| `internal_summary` | low | Internal informational output |
| `send_email` | medium | External communication |
| `publish_content` | medium | Public side effect |
| `update_crm` | medium | Business data mutation |
| `trigger_workflow` | medium | Downstream side effect |
| `apply_discount` | high | Commercial commitment |
| `delete_record` | high | Destructive mutation |
| `financial_change` | high | Financial consequence |
| `account_change` | high | Account/security consequence |
| unknown value | medium | Conservative fallback |

## Approval rule

For this portfolio implementation, **all proposed actions require a human decision**.

That makes the control flow easy to inspect and test.

In production, a business could allow low-risk `draft_only` or `internal_summary` actions to auto-complete while still gating all external side effects.

## Important limitation

Risk is not determined solely by the action label in a production system.

A mature policy would also consider:

- target system
- user role
- monetary value
- customer/account sensitivity
- data classification
- reversibility
- rate limits
- time of day
- confidence thresholds
- prior approvals
