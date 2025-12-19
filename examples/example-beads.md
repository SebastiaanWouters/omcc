# Example Bead Structure

This shows how beads are organized after decomposition.

## Bead Hierarchy

```
bd-001: User Authentication (parent)
├── bd-001.0: Context Brief
├── bd-001.1: User Schema & Types
├── bd-001.2: Registration Endpoint
├── bd-001.3: Login Endpoint
├── bd-001.4: Tests: Happy Path
├── bd-001.5: Tests: Edge Cases
├── bd-001.6: Tests: Error Handling
└── bd-001.9: Verification Checklist
```

## Example Bead Content

### bd-001.0: Context Brief

```markdown
## Context
Authentication system for the application. Enables user registration,
login, and session management using JWT tokens.

## Integration Points
- Database: Users table
- API: /auth/* endpoints
- Middleware: Auth verification for protected routes

## Key Decisions
- JWT with 1-hour expiry
- bcrypt for password hashing
- httpOnly cookies for token storage
```

### bd-001.2: Registration Endpoint

```markdown
## Context
Create user registration endpoint that validates input, hashes password,
and stores user in database.

## Scope
- POST /auth/register endpoint
- Input validation (email format, password strength)
- Password hashing with bcrypt
- User creation in database
- Error handling for duplicate emails

## Files
- `src/routes/auth.ts` - Add register route
- `src/models/user.ts` - User creation function

## Acceptance Criteria
- [ ] Valid registration returns 201 with user ID
- [ ] Duplicate email returns 409 Conflict
- [ ] Invalid email format returns 400
- [ ] Weak password returns 400
- [ ] Password is never stored in plaintext

## Dependencies
- Requires: bd-001.1 (schema must exist first)
- Blocks: bd-001.4, bd-001.5, bd-001.6 (tests need endpoint)
```

## Bead Commands

```bash
# List all beads
bd list --json

# Show ready beads (unblocked)
bd ready --json

# Claim a bead
bd update bd-001.2 --status in_progress --assignee "Claude"

# Close a bead
bd close bd-001.2 --reason "Implemented registration endpoint"
```
