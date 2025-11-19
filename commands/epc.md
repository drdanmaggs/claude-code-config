---
description: Explore-Plan-Code-Commit with subagent context isolation
---

# Question-Asking Rules (Apply Throughout)
- Ask questions ONE AT A TIME - wait for answer before asking next question
- Keep questions clear and specific

# Stage 1: What? (Main Agent)
- Ask what the user wants to achieve
- If GitHub issue referenced, read it
- Ask clarifying questions about the GOAL (ONE AT A TIME)
- Output: Clear success statement

# Stage 2: Explore (SUBAGENT: codebase-scanner)
Use the codebase-scanner subagent to:
- Read relevant files in its fresh 200K context
- Build understanding of current implementation
- Save findings to .claude/docs/tasks/codebase-findings.md
- Return 3-5 bullet summary only

# Stage 3: Reflection Checkpoint (Main Agent)
Read the codebase-findings.md summary, then reflect back:
- What you understand we're trying to build
- Key requirements
- Main technical challenges

## Risk Assessment
Evaluate if this task involves:
- Payment processing or financial transactions
- User authentication or authorisation
- Data integrity or privacy concerns
- Critical business logic with multiple edge cases
- Modifications to security-sensitive code
- Complex state management or workflows

**If NO risks identified:** Continue to Stage 4 (Clarify) automatically.

**If YES to any:** Pause and explain:
- Which risks were identified
- Why TDD would be safer here
- Ask: "This has [specific risks]. Should we switch to /tdd for this?"

[WAIT FOR USER CONFIRMATION ONLY IF RISKS FOUND]


# Stage 4: Clarify (Main Agent)
- Ask informed questions ONE AT A TIME based on exploration
- User preferences/decisions only
- NOT "where is X?" questions

# Stage 5: Plan (Main Agent with Sequential Thinking)
- Use sequential thinking
- Consider 2+ approaches
- Explain trade-offs
- Recommend best approach

[WAIT FOR APPROVAL]

# Stage 6: Implement (Main Agent)
- New branch
- Incremental commits
- Tests

# Stage 7: Automated Testing (SUBAGENT: test-validator)

Use test-validator subagent to:
- Read the implemented code in fresh context
- Read test requirements from .claude/docs/tasks/test-plan.md
- Execute appropriate tests (Puppeteer for UI, Supabase queries for data, test suite for logic)
- Save detailed results to .claude/docs/tasks/test-results.md
- Return summary: ✅ PASS / ❌ FAIL with critical issues only

[If FAILED: Main agent reviews findings and fixes issues, then re-triggers test-validator]

# Stage 8: Code Review (SUBAGENT: code-reviewer)
- Use code-reviewer subagent (read-only, fresh context)
- Present findings

# Stage 9: Commit & Document (Main Agent)
- git diff review
- Conventional commits
- Update docs/issues


# Stage 9: Commit & Document (Main Agent)
- git diff review
- Conventional commits
- Update docs/issues

# Stage 10: Record Session History (SUBAGENT: progress-update)
Use progress-update subagent to:
- Record closed issue to project_journey.md
- Record merged PR details
- Add session summary with what was actually built
