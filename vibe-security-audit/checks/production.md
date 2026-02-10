# Production Readiness Checks

These checks matter when you have real users and real money on the line. Skip any that don't apply to the detected stack.

## 21. Consistent API Protection (Severity: 7/10, Fix: 15 min)

Cross-reference all endpoints from check #9. Verify each has: auth, rate limiting, input validation. Flag inconsistencies.

## 22. AI Cost Controls (Severity: 8/10, Fix: 10 min)

Only if AI APIs detected:

```
openai\.chat\.completions
anthropic\.messages
ChatCompletion\.create
generateText
streamText
```

Verify: per-user rate limiting, usage caps, API key not exposed client-side.

## 23. Email Infrastructure (Severity: 5/10, Fix: 15 min)

Only if email sending detected. Flag direct SMTP without an established service:

```
createTransport
nodemailer
smtplib
SMTP\(
```

## 24. Account Deletion / GDPR (Severity: 6/10, Fix: 30 min)

```
delete.*account
deleteUser
removeUser
gdpr
data.*delet
```

Flag if user accounts exist but no deletion mechanism found.

## 25. Backup Strategy (Severity: 6/10, Fix: 15 min)

```
backup
point.in.time.recovery
pg_dump
mongodump
snapshot
```

## 26. Secret Rotation (Severity: 5/10, Fix: 15 min)

Check: are API keys in environment variables (good) or hardcoded (bad)? Any rotation documentation?

## 27. DDoS Protection (Severity: 6/10, Fix: 10 min)

```
cloudflare
vercel\.json
netlify\.toml
AWS::WAF
```

Flag if app is directly exposed with no CDN/proxy.

## 28. Upload Size Limits (Severity: 6/10, Fix: 5 min)

Only if file uploads detected:

```
multer
formidable
busboy
FileReader
input.*type=["']file["']
```

Verify size limits exist:

```
maxFileSize
fileSizeLimit
limits.*fileSize
maxSize
```

Flag if uploads exist without size limits.

## 29. Audit Logging (Severity: 5/10, Fix: 20 min)

```
audit[_-]?log
auditLog
activity[_-]?log
logActivity
```

## 30. Environment Separation (Severity: 7/10, Fix: 5 min)

Search source files (not `.env`) for test keys:

```
sk_test_
pk_test_
test_api_key
sandbox
STRIPE_TEST
```

Flag if test keys appear in source code.
