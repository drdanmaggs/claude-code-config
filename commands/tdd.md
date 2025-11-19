---
description: Test-driven development workflow with GitHub integration
---

# Test-Driven Development Workflow

## Stage 0: Requirements

Ask: **"What are you building? What does 'working' look like?"**

- Read GitHub issue if referenced (including comments)
- Ask clarifying questions until clear
- Confirm acceptance criteria

**Wait for user confirmation.**

## Stage 1: Explore (No Code/Tests Yet)

- Read relevant files
- Identify existing test framework and patterns
- Note dependencies and edge cases
- Summarise findings in 3-5 bullets

## Stage 2: Plan Test Strategy

Use sequential thinking:
- What behaviours need testing?
- What are critical success/failure paths?
- What edge cases must be covered?
- Consider 2+ approaches with trade-offs
- Recommend best strategy

**[WAIT FOR APPROVAL]**

Update GitHub issue with:
- Test approach and test cases
- Acceptance criteria
- Dependencies/questions

## Stage 3: Write Tests

Create branch: `git checkout -b test/[feature-name]`

Write tests checking each requirement:
- Use project's test framework
- Cover happy path, errors, edge cases
- Add comments explaining validations

**Do NOT write implementation code.**

Show all tests to user.

## Stage 4: Verify Tests Fail

Run tests and confirm:
- ALL new tests fail (expected)
- Failures are for right reasons
- Error messages are clear

Show output to user.

## Stage 5: Commit Tests

Ask: **"Ready to commit tests?"**

- `git diff` to review
- `git add [test files]`
- `git commit -m "test: add tests for [feature]"`
- `git push -u origin test/[feature-name]`

## Stage 6: Plan Implementation Strategy

Use `implementation-planner` subagent with sequential thinking:
- Analyse requirements and test suite
- Consider multiple implementation approaches
- Identify potential pitfalls and edge cases
- Recommend best strategy with reasoning

**[WAIT FOR APPROVAL OF STRATEGY]**

## Stage 7: Implement

Switch to: `git checkout -b feat/[feature-name]`
Merge tests: `git merge test/[feature-name]`

Iterate until all tests pass:
1. Write minimal code following approved strategy
2. Run tests, show results
3. **If tests fail unexpectedly:**
   - Use `failure-analyst` subagent with sequential thinking
   - Diagnose root cause objectively
   - Recommend fix approach with reasoning
   - **[SHOW DIAGNOSIS, WAIT FOR APPROVAL]**
4. Commit logical chunks

**Do NOT modify tests. Keep code simple.**

## Stage 8: Verify Implementation

Beyond tests passing:
- Run feature manually
- Check for errors/warnings
- Verify matches requirements

Invite user to verify independently.

## Stage 9: Code Review

Use `code-reviewer` subagent (mandatory):
- Check for test overfitting
- Identify missed edge cases
- Security/performance/maintainability concerns
- Project convention adherence

Present findings. Iterate if needed.

## Stage 10: Finalise & Document

Ask: **"Ready to finalise?"**

1. Review: `git diff main`
2. Commit: `git commit -m "feat: implement [feature]"`
3. Update docs (README, changelog, API docs)
4. Update GitHub issue (mark complete, link commits, note deviations)
5. Push and create PR with description, test links, verification steps

## Stage 11: Summary & History
```
TDD Complete: [feature name]
───────────────────────
Tests: [count] passing
Commits: [test, impl, docs]
Review: [summary]
Issue: [link] ✓
PR: [link] ✓
```

Use progress-update subagent to record this session in project_journey.md.

Suggest next steps if part of larger work.
---

## Rules
- Never skip approval gates
- Always branch (never main)
- Tests before implementation
- Mandatory code review
- Document in GitHub
- Stop and discuss if tests need modification
