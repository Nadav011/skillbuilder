# Security Guidelines

## Security-First Skill Design

All skills must implement defense-in-depth security. Treat every input as potentially malicious.

---

## File Access Controls

### Deny Patterns (MANDATORY)

Every skill MUST include these deny patterns in its security configuration:

```yaml
security:
  deny:
    - ".env"
    - ".env.*"
    - "secrets/**"
    - "*.pem"
    - "*.key"
    - "**/api_keys*"
    - "credentials.json"
    - "serviceAccount*.json"
    - "**/*secret*"
    - "**/*password*"
```

**Why:** Prevents accidental reading of credential files, protecting API keys, passwords, and certificates.

### Additional Patterns by Project Type

**Node.js/JavaScript:**
```yaml
- "node_modules/**"
- ".npmrc"
- "package-lock.json"  # If > 10MB
```

**Python:**
```yaml
- "venv/**"
- ".pypirc"
- "*.pyc"
```

**Cloud:**
```yaml
- "**/.aws/credentials"
- "**/.gcloud/**"
- "**/firebase-adminsdk-*.json"
```

---

## Input Validation

### Rule 1: Validate at Boundaries

**Before:**
```typescript
// UNSAFE: No validation
async function createUser(data: any) {
  await db.users.insert(data);
}
```

**After:**
```typescript
// SAFE: Zod validation at boundary
import { z } from 'zod';

const CreateUserSchema = z.object({
  name: z.string().min(1).max(100),
  email: z.string().email(),
  age: z.number().min(0).max(150)
});

async function createUser(data: unknown) {
  const validated = CreateUserSchema.parse(data);
  await db.users.insert(validated);
}
```

### Rule 2: File Path Sanitization

**Before:**
```typescript
// VULNERABLE: Path traversal attack
const filePath = userInput;  // Could be "../../etc/passwd"
await fs.readFile(filePath);
```

**After:**
```typescript
// SAFE: Validate and sanitize
import path from 'path';

function sanitizePath(userInput: string, baseDir: string): string {
  const normalized = path.normalize(userInput);
  const resolved = path.resolve(baseDir, normalized);
  
  if (!resolved.startsWith(baseDir)) {
    throw new Error('Invalid path: outside allowed directory');
  }
  
  return resolved;
}

const safePath = sanitizePath(userInput, '/allowed/dir');
await fs.readFile(safePath);
```

### Rule 3: Command Injection Prevention

**Before:**
```bash
# VULNERABLE: Command injection
bash -c "git commit -m '$USER_MESSAGE'"
```

**After:**
```bash
# SAFE: Parameterized
git commit -m "$USER_MESSAGE"  # Shell properly escapes
```

**TypeScript:**
```typescript
// SAFE: Use exec with array (no shell)
import { execFile } from 'child_process';

execFile('git', ['commit', '-m', userMessage], (error, stdout) => {
  // Safe: userMessage can't inject commands
});
```

---

## Secret Management

### Never Store Secrets in:
- skill.yaml
- SKILL.md
- references/ files
- scripts/ (unless encrypted)
- Git repository

### Do Store Secrets in:
- Environment variables (.env with .gitignore)
- Secret managers (Vault, AWS Secrets, Supabase Vault)
- Encrypted key stores

### Example Pattern:

**Bad:**
```yaml
# skill.yaml
api_key: "sk-1234567890abcdef"  # NEVER DO THIS
```

**Good:**
```yaml
# skill.yaml
api_key: "${OPENAI_API_KEY}"  # Reference env var
```

```bash
# .env (gitignored)
OPENAI_API_KEY=sk-1234567890abcdef
```

---

## Logging Security

### Safe Logging

**Do Log:**
- User IDs (not usernames/emails)
- Action types (login, create, update)
- Timestamps
- Request IDs
- Error codes (not full stack traces with data)

**Never Log:**
- Passwords (even hashed)
- API keys or tokens
- Credit card numbers
- Personal Identifiable Information (PII)
- Full request/response bodies

**Example:**

```typescript
// BAD
logger.info('User login', { user: { email: 'user@example.com', password: 'secret' } });

// GOOD
logger.info('User login', { userId: user.id, action: 'login' });
```

---

## Security Checklist for New Skills

Before marking skill as complete:

- [ ] Deny patterns configured
- [ ] Input validation implemented (Zod)
- [ ] File paths sanitized (no traversal)
- [ ] Commands parameterized (no injection)
- [ ] Secrets loaded from env vars
- [ ] Logging excludes PII
- [ ] Dependencies scanned (npm audit)
- [ ] Rate limiting applied (if network calls)
- [ ] Error messages don't leak sensitive data
- [ ] Tests include security test cases

**Golden Rule:** If uncertain, ASK. Never assume a security practice is unnecessary.
