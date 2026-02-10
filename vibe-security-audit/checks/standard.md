# Standard Checks

Foundation and common mistake checks. Skip any that don't apply to the detected stack.

## 2. Ghost Packages (Severity: 7/10, Fix: 5 min)

Review `package.json` or `requirements.txt`. For each unfamiliar dependency, verify it exists on npm/PyPI and has reasonable download counts. Flag packages with < 100 weekly downloads.

## 4. Security Self-Review (Severity: 3/10, Fix: 10 min)

Search for evidence the code has been security-reviewed:

```
TODO.*security
FIXME.*security
SECURITY
security.*test
test.*security
```

## 7. Outdated Dependencies (Severity: 7/10, Fix: 5 min)

Run the appropriate audit command:
- npm: `npm audit`
- pip: `pip audit`
- go: `govulncheck ./...`
- ruby: `bundle audit`

Flag high/critical severity issues.

## 8. Rate Limiting (Severity: 7/10, Fix: 10 min)

Search for rate limiting middleware:

```
rate-limit
rateLimit
express-rate-limit
@upstash/ratelimit
throttle
```

Flag if no rate limiting found AND the project has API routes.

## 9. Attack Surface (Severity: 6/10, Fix: varies)

Find all route/endpoint definitions:

```
# Express/Node
app\.(get|post|put|patch|delete)\s*\(
router\.(get|post|put|patch|delete)\s*\(

# Next.js API routes
export\s+(async\s+)?function\s+(GET|POST|PUT|PATCH|DELETE)

# Python Flask/FastAPI
@app\.(get|post|put|patch|delete)\s*\(
@router\.(get|post|put|patch|delete)\s*\(

# Astro/SvelteKit
export\s+(async\s+)?function\s+(GET|POST|PUT|DELETE|PATCH)
```

Verify each has: auth check, input validation, error handling.

## 10. Row Level Security (Severity: 9/10, Fix: 10 min)

Only if Supabase/PostgreSQL detected. Search for:

```
\.from\(
supabase\.from
CREATE POLICY
ALTER TABLE.*ENABLE ROW LEVEL SECURITY
```

Flag as critical if Supabase is used but no RLS policies found.

## 12. Open Redirects (Severity: 6/10, Fix: 5 min)

```
[?&](redirect|next|returnUrl|return_to|callback|continue|dest|goto|url)=
redirect\s*\(\s*req\.(query|params|body)
res\.redirect\s*\(\s*req\.
window\.location\s*=\s*.*searchParams
router\.push\s*\(\s*.*searchParams
```

## 13. Storage Bucket Permissions (Severity: 8/10, Fix: 10 min)

Only if cloud storage detected:

```
publicRead
public-read
AllUsers
allUsers
acl.*public
"public":\s*true
```

## 14. Debug Statements (Severity: 5/10, Fix: 5 min)

Focus on debug output logging sensitive data:

```
console\.log\s*\(.*user
console\.log\s*\(.*token
console\.log\s*\(.*password
console\.log\s*\(.*secret
console\.log\s*\(.*auth
console\.log\s*\(.*session
console\.log\s*\(.*credential
print\s*\(.*password
print\s*\(.*token
```

Don't flag `console.error` in catch blocks.

## 15. Webhook Verification (Severity: 8/10, Fix: 5 min)

Only if webhooks detected. Search for webhook endpoints:

```
webhook
/api/webhook
/api/stripe
```

Then verify signature checking exists nearby:

```
constructEvent
verify.*signature
verifyWebhookSignature
svix.*verify
```

Flag as critical if webhook endpoint exists without signature verification.

## 16. Server-Side Permission Checks (Severity: 8/10, Fix: 10 min)

Cross-reference API routes from check #9 with auth patterns:

```
getServerSession
getSession
auth\(\)
currentUser
requireAuth
isAuthenticated
verifyToken
```

Flag any API route without at least one auth pattern.

## 17. Dependency Vulnerabilities (Severity: 7/10, Fix: 5 min)

Run the actual audit command from check #7 if not already done.

## 18. Password Reset Security (Severity: 7/10, Fix: 10 min)

Only if password reset exists:

```
forgot[-_]?password
reset[-_]?password
password[-_]?reset
resetToken
```

Verify: rate limiting on endpoint, token expiration < 1 hour, single-use tokens.

## 19. Error Leakage (Severity: 6/10, Fix: 5 min)

```
res\.(json|send)\s*\(\s*\{?\s*error:\s*err
res\.(json|send)\s*\(\s*err\.message
res\.(json|send)\s*\(\s*err\.stack
stack:\s*err\.stack
traceback\.format_exc
```

## 20. Session Management (Severity: 7/10, Fix: 5 min)

```
expiresIn
maxAge
token.*expir
session.*expir
cookie.*maxAge
```

Flag if no expiration is set on auth tokens/sessions.
