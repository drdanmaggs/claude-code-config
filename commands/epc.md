---
description: Explore-Plan-Code-Commit with intelligent issue scoping
---

# Stage 1: Check Starting Point

Does the user have a scoped issue?
- Look for: issue number, pasted issue content, or clear acceptance criteria
- If YES → Go to Stage 3 (Implementation Planning)
- If NO → Go to Stage 2 (Get Scope)

# Stage 2: Get Scope (SUBAGENT: issue-scoper)

Say: "Let me scope this properly first."

Call `issue-scoper` subagent to:
- Understand the feature
- Explore the codebase
- Create INVEST-compliant issue(s)
- Return structured scope

When issue-scoper returns:
- If multiple issues: Ask "Which should we implement first?"
- If single issue: Say "Here's the scope. Ready to implement?"

Save selected issue to `.claude/docs/tasks/current-issue.md` for reference.

# Stage 3: Implementation Planning (Main Agent)

Working from the issue (either provided or from issue-scoper):
- Use sequential thinking to consider approaches
- Map each acceptance criterion to implementation steps
- Consider 2+ approaches with trade-offs
- Recommend best approach

[WAIT FOR APPROVAL]

# Stage 4: Risk Check

Evaluate if this involves:
- Payment/financial transactions
- Authentication/authorization
- Data privacy concerns
- Complex business logic

If risks found:
- Ask: "This has [risks]. Should we use /tdd instead?"
- [WAIT FOR ANSWER]

# Stage 5: Implementation (Main Agent)

Create feature branch:
```bash
git checkout -b feat/[brief-description]
```

Implement the approved plan:
- Address each acceptance criterion
- Write tests alongside code
- Commit logical chunks

# Stage 6: Code Review (SUBAGENT: code-reviewer)

Call code-reviewer to:
- Check against acceptance criteria
- Review code quality
- Verify test coverage

Fix any critical issues.

# Stage 7: Wrap Up

Verify all acceptance criteria met:
```
✓ [Criterion 1] - DONE
✓ [Criterion 2] - DONE  
✓ [Criterion 3] - DONE
```

Final commit and push:
```bash
git add -A
git commit -m "feat: [description]"
git push -u origin feat/[branch]
```

Ask: "Ready to create PR?"