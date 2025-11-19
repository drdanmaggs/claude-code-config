---
description: Review changes and create conventional commit
---

1. Run `git status` to see what's changed
2. Run `git diff` to review the actual changes
3. Determine commit type:
   - feat: New feature
   - fix: Bug fix
   - docs: Documentation
   - style: Formatting, no code change
   - refactor: Code restructuring
   - test: Adding tests
   - chore: Maintenance
4. Stage changes: `git add -A`
5. Create commit with format: `type: clear description`
6. Ask before pushing: "Ready to push to [branch]?"