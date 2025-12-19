# Plan: User Authentication

## Problem Statement
Users need to log in securely to access protected resources.

## Goals
- [ ] Users can register with email/password
- [ ] Users can log in and receive a session token
- [ ] Protected routes require valid authentication

## Non-Goals
- OAuth/social login (future phase)
- Password reset flow (separate plan)

## Approach
JWT-based authentication with bcrypt password hashing. Tokens stored in httpOnly cookies.

### Key Decisions
- JWT over sessions: Stateless, scales horizontally
- bcrypt over argon2: Wider library support
- Cookies over localStorage: XSS protection

## Phases

### Phase 1: User Model & Registration
**Scope**: Database schema, registration endpoint
**Files Affected**:
- `src/models/user.ts` - User schema with password hashing
- `src/routes/auth.ts` - Registration endpoint
- `tests/auth.test.ts` - Registration tests

### Phase 2: Login & Token Generation
**Scope**: Login endpoint, JWT generation
**Files Affected**:
- `src/routes/auth.ts` - Login endpoint
- `src/utils/jwt.ts` - Token generation/validation
- `tests/auth.test.ts` - Login tests

### Phase 3: Protected Routes
**Scope**: Auth middleware, route protection
**Files Affected**:
- `src/middleware/auth.ts` - JWT validation middleware
- `src/routes/protected.ts` - Example protected route

## Testing Strategy
- Unit tests for password hashing and JWT functions
- Integration tests for auth endpoints
- Manual testing with Postman/curl

## Risks & Mitigations
| Risk | Mitigation |
|------|------------|
| Token leakage | Use httpOnly cookies, short expiry |
| Brute force | Rate limiting on login endpoint |

## Open Questions
- Token expiry duration? (Propose: 1 hour access, 7 day refresh)
