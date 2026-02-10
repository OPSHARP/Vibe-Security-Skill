# Critical Checks

Run these 5 checks first. They catch the most damaging vulnerabilities.

## 1. Secrets Exposure (Severity: 10/10, Fix: 2 min)

Search all source files (exclude `node_modules`, `dist`, `.next`, `build`, `vendor`):

```
sk-[a-zA-Z0-9]{20,}
sk_live_[a-zA-Z0-9]+
ghp_[a-zA-Z0-9]{36}
github_pat_[a-zA-Z0-9_]{60,}
AKIA[0-9A-Z]{16}
xox[baprs]-[a-zA-Z0-9-]+
(api[_-]?key|api[_-]?secret|password|passwd|secret|token|auth[_-]?token)\s*[:=]\s*["'][^"']{8,}["']
(postgres|mysql|mongodb|redis)://[^/\s]+:[^/\s]+@
Bearer\s+[a-zA-Z0-9._-]{20,}
-----BEGIN (RSA |EC |DSA )?PRIVATE KEY-----
```

Also verify:
- `.env` is in `.gitignore`
- No `.env` files tracked: `git ls-files '*.env*'`

## 3. Custom Auth (Severity: 9/10, Fix: 15 min)

Search for hand-rolled auth patterns:

```
createHash\s*\(\s*['"]md5['"]
createHash\s*\(\s*['"]sha1['"]
createHash\s*\(\s*['"]sha256['"]
hashlib\.md5
hashlib\.sha1
jwt\.sign\s*\(
jsonwebtoken.*sign
req\.session\.user\s*=
session\[['"]user['"]\]\s*=
localStorage\.setItem\s*\(.*token
```

Flag if none of these auth providers appear in dependencies: `@clerk`, `next-auth`, `@auth/`, `@supabase/auth`, `firebase`, `@auth0`, `lucia`, `passport`.

## 5. Injection / Unsanitized Input (Severity: 10/10, Fix: 5 min)

```
# Raw SQL with interpolation (JS/TS)
query\s*\(\s*`[^`]*\$\{
query\s*\(\s*['"][^'"]*['"]\s*\+
execute\s*\(\s*`[^`]*\$\{
\.raw\s*\(\s*`[^`]*\$\{

# Raw SQL with interpolation (Python)
execute\s*\(\s*f["']
cursor\.execute\s*\([^,)]*\+
cursor\.execute\s*\(.*\.format\(

# Dangerous functions
eval\s*\(
child_process\.exec\s*\(.*req\.
Function\s*\(\s*['"]return
dangerouslySetInnerHTML
v-html\s*=
```

## 6. Gitignore Coverage (Severity: 8/10, Fix: 1 min)

Verify `.gitignore` includes all of:

```
.env
.env.*
.env.local
.env.production
node_modules/
__pycache__/
venv/
dist/
build/
.next/
.nuxt/
.DS_Store
Thumbs.db
.idea/
*.log
*.pem
*.key
credentials.json
service-account*.json
```

## 11. CORS Misconfiguration (Severity: 8/10, Fix: 2 min)

```
cors\(\s*\)
origin:\s*['"][*]['"]
origin:\s*true
Access-Control-Allow-Origin.*[*]
origin:\s*req\.headers\.origin
```
