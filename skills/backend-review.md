# Backend Review

Comprehensive backend code review for Python/Django APIs, services, and infrastructure.

## When to Use

- After implementing new API endpoints
- Before deploying to production
- When reviewing backend pull requests
- After security incidents or audits
- When performance degrades
- Before major refactoring

## Review Checklist

### 1. Security (OWASP Top 10)

- **Injection**: Are SQL/NoSQL queries parameterized? (Django ORM default, but check raw queries)
- **Authentication**: Are auth mechanisms secure? (Django's built-in auth, JWT, OAuth)
- **Sensitive Data**: Are secrets/credentials managed via environment variables? (never hardcoded)
- **Access Control**: Are permissions checked at every endpoint? (use decorators or middleware)
- **Security Misconfiguration**: Are defaults secure? (DEBUG=False, ALLOWED_HOSTS set)
- **XSS/CSRF**: Are cross-site attacks prevented? (Django's CSRF middleware enabled)
- **Deserialization**: Is untrusted data validated before deserialization? (use serializers)
- **Logging**: Are sensitive data excluded from logs? (no PII, passwords, tokens)
- **Rate Limiting**: Are endpoints protected from abuse? (use rate limiting middleware)

### 2. Django-Specific

- **Querysets**: Are they lazy-loaded appropriately? (avoid premature evaluation)
- **N+1 Prevention**: Is select_related/prefetch_related used?
  - select_related for foreign keys (1-to-1, many-to-1)
  - prefetch_related for reverse foreign keys and many-to-many
- **Signals**: Are they used sparingly? (avoid hidden side effects, prefer explicit calls)
- **Migrations**: Are they reversible? (provide reverse operations for data migrations)
- **Admin Panel**: If exposed, is it properly secured? (non-standard URL, authentication required)
- **Views**: Are CBVs used for CRUD, FBVs for custom logic?
- **Transactions**: Are database writes wrapped in atomic transactions where needed?

### 3. Python-Specific

- **Type Hints**: Are they used? (Python 3.11+ syntax, all function signatures)
- **String Formatting**: Are f-strings used? (not % or .format())
- **Context Managers**: Are resources managed with with statements? (files, connections)
- **Pathlib**: Is pathlib used instead of os.path?
- **Dataclasses**: Are they used for structured data? (instead of dicts or tuples)
- **List Comprehensions**: Are they preferred over map/filter where readable?

### 4. Error Handling

- Are errors caught and logged appropriately? (structured logging)
- Are error messages safe for users? (no stack traces or internal details in production)
- Are database errors handled gracefully? (retry transient errors)
- Are network failures retried with backoff? (exponential backoff for external APIs)
- Are timeouts configured? (avoid hanging requests)
- Are edge cases handled? (null checks, empty lists, divide-by-zero)

### 5. Database & Data

- **Queries Optimized**: Are indexes in place for frequently queried fields?
- **Transactions**: Are they used where needed? (multi-step operations)
- **Validation**: Is data validated before persistence? (use serializers and forms)
- **Migrations**: Are they reversible? (provide reverse operations)
- **Connection Pooling**: Is it configured? (connection settings)
- **Data Sanitization**: Is user input sanitized before storage?
- **Foreign Keys**: Are constraints in place? (on_delete behavior defined)

### 6. API Design

- **REST Conventions**: Are endpoints RESTful? (nouns for resources, verbs for actions)
- **HTTP Methods**: Are they used correctly? (GET: read, POST: create, PUT/PATCH: update, DELETE: delete)
- **Status Codes**: Are they appropriate?
  - 200: OK, 201: Created, 204: No Content
  - 400: Bad Request, 401: Unauthorized, 403: Forbidden, 404: Not Found
  - 500: Internal Server Error, 502: Bad Gateway, 503: Service Unavailable
- **Responses**: Are they consistent? (same structure for success/error)
- **Pagination**: Is it implemented for large datasets? (use pagination classes)
- **Versioning**: Is it in place? (/api/v1/, /api/v2/)
- **Schemas**: Are request/response schemas documented? (OpenAPI/Swagger)

### 7. Performance

- **Caching**: Are expensive operations cached? (Redis, Memcached, or framework cache)
- **Database Queries**: Are they minimized? (batch operations, avoid loops with queries)
- **Background Jobs**: Are slow tasks async? (Celery, task queues)
- **Resources**: Are they cleaned up? (database connections, file handles)
- **Memory Leaks**: Are they prevented? (watch for global state accumulation)
- **Concurrency**: Is it handled properly? (thread-safe code, avoid shared mutable state)

### 8. Testing

- **Critical Paths**: Are they tested? (happy path + edge cases)
- **Mocks/Stubs**: Are they used appropriately? (mock external APIs, not database)
- **Integration Tests**: Are they in place? (test client for API endpoints)
- **Database Tests**: Are they isolated? (use test transactions or test case classes)
- **Coverage**: Is it reasonable? (>80% for critical code)
- **Test Data**: Is it realistic? (use factories for test data)

### 9. Code Quality

- **Single Responsibility**: Are functions/classes focused on one task?
- **Magic Values**: Are they extracted to constants or config? (settings or environment)
- **DRY**: Is code duplicated unnecessarily?
- **Naming**: Are names clear and consistent? (descriptive, not abbreviated)
- **Comments**: Are they necessary and accurate? (explain "why", not "what")
- **Technical Debt**: Is it documented? (TODO comments with context)

### 10. Observability

- **Metrics**: Are they collected? (request count, latency, error rate)
- **Logs**: Are they structured and searchable? (JSON logs with context)
- **Traces**: Are they included for debugging? (request IDs, user IDs)
- **Alerts**: Are they configured for critical failures? (500 errors, slow queries)
- **Monitoring**: Is it in place? (error tracking, performance monitoring)

### 11. Reliability

- **Graceful Failures**: Are they handled? (circuit breakers, retries)
- **Idempotency**: Is it enforced where needed? (POST endpoints that create resources)
- **Distributed Transactions**: Are they handled correctly? (use idempotency keys)
- **Race Conditions**: Are they prevented? (select_for_update, database constraints)
- **Data Consistency**: Is it guaranteed? (transactions, ACID properties)

### 12. Infrastructure

- **Resources**: Are they properly sized? (CPU, memory, disk)
- **Autoscaling**: Are rules configured? (horizontal scaling for web servers)
- **Backups**: Are they in place? (automated database backups)
- **Disaster Recovery**: Is it planned? (restore procedures documented)
- **Costs**: Are they optimized? (right-sized instances, reserved capacity)

## Output Format

```markdown
## Backend Review: [Service/Feature Name]

### Critical Security Issues 🔴
- **[OWASP Category]**: [Vulnerability]
  - **Location**: `path/to/file.py:42`
  - **Risk**: [Impact if exploited]
  - **Fix**: [Remediation steps]
  - **Example Attack**: [How this could be exploited]

### Data/Database Issues 🗄️
- **[Issue Type]**: [Description]
  - **Location**: `path/to/file.py:15`
  - **Impact**: [Performance/correctness impact]
  - **Fix**: [Suggested solution]

### API Design Issues 📡
- **[Issue]**: [Description]
  - **Current**: [What's wrong]
  - **Expected**: [Best practice]
  - **Impact**: [Why this matters]

### Performance Concerns ⚡
- **[Issue]**: [Description]
  - **Measurement**: [Current performance]
  - **Optimization**: [How to improve]
  - **Expected Gain**: [Performance improvement]

### Warnings 🟡
- **[Category]**: [Description]
  - **Location**: `path/to/file.py:28`
  - **Suggestion**: [How to improve]

### Testing Gaps 🧪
- **[Missing Tests]**: [Description]
  - **Critical Paths**: [What needs testing]
  - **Edge Cases**: [Scenarios to cover]

### Questions ❓
- [Question 1]: [Clarification needed]

### Verified ✅
- [Aspect 1]: [What's done well]
- [Security Control]: [Properly implemented]
```

## Important

- **Security First**: Flag all security issues as critical (block merging)
- **Data Integrity**: Ensure database operations are safe and correct
- **Performance**: Consider scale (what breaks at 10x traffic?)
- **Errors**: Verify all failure modes are handled
- **Tests**: Check that tests actually validate correctness (not just coverage)
- **Logging**: Ensure sensitive data isn't logged (PII, passwords, tokens)

## Common Anti-Patterns to Catch

### Django-Specific
- N+1 query problems (missing select_related/prefetch_related)
- Unbounded list endpoints (no pagination)
- Using signals for critical business logic (hard to debug)
- Not using atomic transactions for multi-step operations
- Exposing admin panel without proper security
- Overusing raw SQL (use ORM when possible)

### Python-Specific
- Missing type hints (makes code hard to understand)
- Using % or .format() instead of f-strings
- Not using context managers for resources
- Mutable default arguments (function default arguments should be immutable)
- Using os.path instead of pathlib

### API-Specific
- Missing input validation (trust user input)
- Hardcoded secrets (credentials in code)
- Lack of idempotency for critical operations
- Missing rate limiting (API abuse)
- Returning 200 for errors (should be 4xx/5xx)

### Database-Specific
- Missing indexes on frequently queried fields
- SQL injection via string concatenation (use parameterized queries)
- Not handling database connection failures
- Missing foreign key constraints (data integrity issues)
- Unbounded queries (no LIMIT clause)

## Verification Steps

Before marking review complete:

1. **Run the code** (if possible) — does it work?
2. **Check tests** — do they pass? Do they test the right things?
3. **Review migrations** — are they reversible? Safe to run in production?
4. **Check logs** — any errors or warnings?
5. **Verify security** — any OWASP Top 10 vulnerabilities?
6. **Measure performance** — any slow queries or endpoints?
