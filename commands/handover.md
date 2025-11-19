---
description: Save current progress to GitHub issue for context recovery
---

You are about to lose context due to errors or context exhaustion. Before we clear:

1. Review our current conversation to identify:
   - What we were trying to achieve
   - Progress made so far (completed steps)
   - Current blockers or errors
   - Exact state of the codebase (files changed, commits made)
   - Next steps we had planned

2. Create a GitHub issue comment that includes:
   - **Objective**: What we're building/fixing
   - **Progress**: What's working, what's been completed
   - **Current State**: Files modified, last working commit
   - **Blocker**: Specific error or problem we hit
   - **Context**: Any crucial technical decisions or approaches
   - **Next Steps**: What to try after context clear

3. Format the comment so another session can pick this up immediately with zero ambiguity

4. Post the comment to the current GitHub issue

Write it like you're handing off to a colleague who needs to resume your work.
```

## Usage

Once saved, just type:
```
/handover