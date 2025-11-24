---
description: Parse Claude's automated PR reviews and fix relevant issues for MVP
---

# PR Feedback Workflow For MVP Stage Work

---
description: Parse Claude's automated PR reviews and fix relevant issues for MVP
---

# PR Feedback Workflow For MVP Stage Work

## Stage 1: Fetch Claude's Review

**Get the bot review:**

1. Use Bash to fetch PR comments (same as /pr-comments):
```bash
   gh api /repos/{owner}/{repo}/issues/{pr_number}/comments
```
2. Find the latest comment by user login 'github-actions[bot]' or 'claude[bot]'
3. Parse the structured review sections from comment body
4. Say: "Found Claude's review, parsing feedback..."

**If no Claude review yet:**
- Say: "No Claude review found yet. Has the bot finished reviewing?"
[EXIT]

## Stage 2: Get PR Details

Before fetching comments, get the PR number and repo details:
```bash
# If user provides PR URL, extract number
# If user provides PR number, use directly
# Get current repo details:
gh pr view {pr_number} --json number,headRepository
```

This gives you:
- PR number confirmation
- Owner/repo names
- Branch details

## Stage 3: Fetch Comments

Use the reliable Bash approach:
```bash
gh api /repos/{owner}/{repo}/issues/{pr_number}/comments
```

Look for comment with:
- `user.login` = "github-actions[bot]" or "claude[bot]"
- `body` containing review sections
- Most recent by `created_at` timestamp

## Stage 4: Parse Review Sections

Extract items and categorise:

- ⚠️ Security = Must fix
- 🐛 Bugs = Must fix  
- 🧹 Quick tidy = Fix if <2 minutes
- 💡 Suggestions = Skip unless trivial
- 📚 Tests/docs = Skip for MVP

**Quick tidy includes:**
- Orphaned/unused files or components
- Duplicate/redundant code blocks
- Dead imports
- Console.log statements left in
- Commented-out code blocks
- Obvious typos in user-facing text

## Stage 5: Triage for MVP

**Always fix:**
- Security vulnerabilities
- Actual bugs
- Build/lint/type errors

**Quick tidy (if <2 minutes each):**
- Delete unused components/files
- Remove duplicate code blocks
- Clean up dead imports
- Remove console.logs
- Fix obvious typos

**Skip for MVP:**
- "Add comprehensive tests"
- Complex refactoring
- Performance optimisation
- "Consider X pattern"

## Stage 6: Implement Fixes

For each issue to fix:

1. **Security fixes** (like XSS):
   - Add sanitization
   - Don't overthink - use DOMPurify or remove HTML

2. **Performance quick wins**:
   - Replace `<img>` with `next/image` 
   - Add priority to hero images

3. **Bug fixes**:
   - Fix the specific issue
   - Don't refactor everything

Use sequential thinking only if trade-offs exist.

## Stage 7: Verify Fixes Work

**Before committing, verify nothing broke:**

1. **Run build:**
   ```bash
   npm run build
   ```
   - If FAILS: Stop and fix the build error first
   - If PASSES: Continue

2. **Run linter:**
   ```bash
   npm run lint
   ```
   - If FAILS: Fix linting errors
   - If PASSES: Continue

3. **Run type check (if TypeScript):**
   ```bash
   npm run typecheck || npx tsc --noEmit
   ```
   - If FAILS: Fix type errors
   - If PASSES: Ready to commit

Say: "✅ Build, lint, and type checks pass - fixes verified"

**If any checks fail:**

- Fix the issues
- Re-run all checks
- Only proceed when all pass

## Stage 8: Commit and Respond

After fixes AND verification:

1. Commit: `fix: address security and bug fixes from PR review`
2. Push to branch
3. Add comment on PR:
```
✅ Addressed critical feedback:
- Fixed XSS vulnerability with HTML sanitization
- [Other fixes]

📝 Acknowledged for post-MVP:
- Comprehensive test suite
- Static generation optimization
- [Other deferrals]

Ready to merge for MVP.
```

## Decision Framework

**Claude suggests "Consider X"?**
- Is it broken without it? → Fix
- Will users notice? → Maybe fix
- Is it "best practice"? → Skip

**Claude shows code examples?**
- Is it a security fix? → Copy and use
- Is it an optimization? → Skip unless trivial
- Is it a "better pattern"? → Skip

**Claude mentions "project-wide issue"?**
- Skip it - not this PR's problem

## Common Claude Bot Patterns

Claude often suggests:
1. **"Use next/image instead of img"** → Quick win, do it
2. **"Add generateStaticParams"** → Skip for MVP
3. **"Consider adding tests"** → Always skip
4. **"Security: sanitize HTML"** → ALWAYS fix
5. **"Add loading states"** → Skip unless broken
6. **"Implement metadata for SEO"** → Skip for MVP

## Your Context

- You're shipping solo with Claude as reviewer
- No human reviewers to argue with
- Claude can be overly thorough (trained on enterprise code)
- Your bar: "Does it work for users?" not "Is it perfect?"