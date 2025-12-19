# Safety Rules

These rules apply globally to prevent destructive actions.

## File Deletion — FORBIDDEN

- **NEVER** delete any file without explicit user permission in this session
- This includes files you just created (tests, scripts, temp files)
- You do not get to decide something is "safe" to remove
- If you think something should be deleted, **STOP AND ASK**

## Destructive Git Commands — FORBIDDEN

The following commands require **explicit user approval** with the exact command in the same message:

- `git reset --hard`
- `git clean -fd`
- `git push --force` (or `-f`)
- `git rebase` (interactive)
- `rm -rf`
- Any command that can delete or overwrite code/data permanently

If a user asks for a destructive action, confirm:
```
This will permanently delete/overwrite data.
Exact command: <command>
Type 'yes' to confirm.
```

## Bulk Code Modifications — FORBIDDEN

- No codemods
- No one-off regex replacement scripts
- No giant sed/awk refactors across multiple files
- No automated find-and-replace across the codebase

For large mechanical changes:
- Break into smaller explicit edits
- Edit file-by-file
- Get user approval for each significant change

## Secret Files — FORBIDDEN

Never commit or display contents of:
- `.env` files
- `*credentials*` files
- `*secret*` files
- API keys, tokens, passwords

If you encounter these, warn the user and do not proceed.

## Package Installation — CAUTION

Before installing packages:
- Verify package name is correct (typosquatting protection)
- Check if package is already in dependencies
- Prefer well-maintained, popular packages

## External API Calls — CAUTION

Before making API calls:
- Ensure the endpoint is trusted
- Never send sensitive data without explicit approval
- Prefer official SDK/library methods over raw HTTP

## Error Recovery

If you accidentally:
- Delete something: Immediately attempt `git checkout` or `git stash`
- Make a bad commit: Inform user immediately, do NOT attempt to fix silently
- Break the build: Stop and inform user before any more changes
