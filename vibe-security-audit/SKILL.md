---
name: vibe-security-audit
description: Audits codebases for security vulnerabilities common in AI-assisted (vibe coded) projects. Runs a 30-point checklist covering secrets exposure, injection attacks, auth misconfig, CORS, dependency vulnerabilities, and production readiness. Use when reviewing code for security issues, preparing for deployment, or when the user mentions security audit, vulnerability check, or vibe coding security.
license: MIT
metadata:
  author: Kedasha | @itsthatladydev
  version: "3.0"
---

# Vibe Security Audit

Perform a 30-point security audit on the target code. If an argument is provided, focus on `$ARGUMENTS`. Otherwise, audit the entire project.

## Contents

- **Critical checks (1, 3, 5, 6, 11)**: See [checks/critical.md](checks/critical.md)
- **Foundation & common checks (2, 4, 7-10, 12-20)**: See [checks/standard.md](checks/standard.md)
- **Production readiness checks (21-30)**: See [checks/production.md](checks/production.md)

## Step 1: Detect Tech Stack

Identify what to check by examining these files:

| Detect          | Look for                                                              |
|:----------------|:----------------------------------------------------------------------|
| Framework       | `next.config.*`, `nuxt.config.*`, `astro.config.*`, `vite.config.*`   |
| Package manager | `package.json`, `requirements.txt`, `go.mod`, `Gemfile`              |
| Database        | `prisma/`, `drizzle.config.*`, `supabase/`, `firebase.json`          |
| Auth            | Imports: `@clerk`, `next-auth`, `@supabase/auth`, `@auth0`, `lucia`  |
| Storage         | Imports: `@aws-sdk/client-s3`, `@supabase/storage`, `@google-cloud/storage` |
| AI APIs         | Imports: `openai`, `@anthropic-ai/sdk`, `@google/generative-ai`      |
| Payment         | Imports: `stripe`, `@paddle/paddle-node`                              |
| Email           | Imports: `resend`, `@sendgrid/mail`, `nodemailer`                     |
| Deployment      | `vercel.json`, `netlify.toml`, `fly.toml`, `Dockerfile`              |

Skip checks that don't apply to the detected stack.

## Step 2: Run Audit

Copy this checklist and check off items as you complete them:

```
Audit Progress:
- [ ] Step 1: Detect tech stack
- [ ] Step 2a: Run critical checks (1, 3, 5, 6, 11) — see checks/critical.md
- [ ] Step 2b: Run standard checks (2, 4, 7-10, 12-20) — see checks/standard.md
- [ ] Step 2c: Run production checks (21-30) — see checks/production.md
- [ ] Step 3: Calculate score and compile report
```

**Run critical checks first** — read [checks/critical.md](checks/critical.md) and execute all 5 checks before proceeding.

Then read [checks/standard.md](checks/standard.md) and [checks/production.md](checks/production.md) for remaining checks. Skip any that don't apply to the detected stack.

## Step 3: Score and Report

### Severity Scale

| Score  | Level         | Action                  |
|:-------|:--------------|:------------------------|
| 10/10  | Critical      | Fix before deploying    |
| 8-9/10 | High          | Fix within 24 hours     |
| 6-7/10 | Medium        | Fix within 1 week       |
| 4-5/10 | Low           | Fix when convenient     |
| 1-3/10 | Informational | Consider addressing     |

**Project score** = 100 minus sum of severity scores for all issues found (minimum 0).

| Score  | Rating                  |
|:-------|:------------------------|
| 90-100 | Excellent               |
| 70-89  | Good — minor issues     |
| 50-69  | Fair — needs attention  |
| 30-49  | Poor — significant risk |
| 0-29   | Critical — do not deploy|

### Output Format

```markdown
# Security Audit Report

## Detected Tech Stack
- **Framework**: [detected]
- **Database**: [detected]
- **Auth**: [detected]
- **Deployment**: [detected]
- **Other**: [AI APIs, payment, email, storage if detected]

## Summary
[1-2 sentence overview]

**Project Score**: [X/100] — [Rating]
**Checks Run**: [X/30]
**Issues Found**: [count]

## Quick Wins (fixable in under 10 minutes)
| # | Issue | Severity | Fix Time | Fix |
|---|-------|----------|----------|-----|
| 1 | [issue] | [X/10] | [time] | [one-line fix] |

## Critical Issues (Severity 8-10)
- **[Check Name]** (Severity: X/10)
  - File: [file:line]
  - Pattern matched: [what was found]
  - Fix: [specific remediation]

## High Priority (Severity 6-7)
[Same format]

## Medium Priority (Severity 4-5)
[Same format]

## Low Priority (Severity 1-3)
[Same format]

## Passed Checks
[Check number and name for each passing check]

## Not Applicable
[Check number, name, and reason skipped]
```

Reference exact files and line numbers. Show which pattern matched. Provide actionable fix instructions for every issue.
