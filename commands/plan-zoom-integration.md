---
description: Turn a Zoom product idea into a practical build plan with auth, architecture, milestones, and risk calls.
---

# Plan Zoom Integration

Use this command when the right Zoom surface is mostly clear and the next step is a concrete delivery plan.

## Preflight

1. Inspect the repository for current Zoom code, deployment shape, env templates, auth paths, and event or media handling.
2. Capture the target user flow, success criteria, and the concrete business outcome.
3. Identify the owning Zoom surface or surfaces and any external systems the integration must connect to.
4. Note hard constraints early: platform, OAuth app type, webhook reachability, SDK environment limits, or marketplace review assumptions.
5. Select the Marketplace or Build Platform app model and scenario template before finalizing auth, event, or SDK-signature work.

## Plan

Before producing the build plan:

- state the Zoom surfaces in scope
- state the expected auth and event model
- state the major implementation phases
- state whether the plan is greenfield, retrofit, or repair on top of an existing integration

## Commands

1. Inventory the current repo and identify which parts can be reused versus what must be added.
2. Define the integration architecture: frontend, backend, auth owner, event flow, and any live media components.
3. Define the required Zoom app type, template, scopes, callback paths, products, events, and token lifecycle expectations.
4. Route app creation and manifest validation through `/setup-zoom-marketplace-app`.
5. Break the implementation into phases: app registration, smallest working path, reliability, observability, and polish.
6. Call out the highest-risk dependencies early instead of burying them under tasks.
7. End with the smallest deliverable that proves the architecture works.

## Verification

1. Confirm the plan includes architecture, auth, scopes, implementation phases, and top risks.
2. Verify the proposed sequence matches the repo’s current state and the user’s goal.
3. Distinguish confirmed repo facts from assumptions or unresolved questions.

## Summary

```text
## Result
- Action: produced a Zoom integration build plan
- Status: success | partial | failed
- Details: architecture, auth model, phases, risks, first milestone
```

## Next Steps

- Run the most relevant build or setup command for phase one.
- If the owning surface is still unclear, run `/plan-zoom-product`.
- If the repo already fails in one layer, run `/debug-zoom` before implementing further.
