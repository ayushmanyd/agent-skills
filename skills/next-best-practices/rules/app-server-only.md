# app-server-only - Use server-only Package

**Priority:** CRITICAL  
**Category:** App Router & Server Components  
**Version:** Next.js 15+

### Why It Matters

The `server-only` package prevents server-side code from accidentally being included in client bundles, protecting sensitive data and reducing bundle size.

### Incorrect

```tsx
// lib/database.ts
// No protection - could be imported in client components

import { db } from "./db-connection";

export async function getUser(id: string) {
  return await db.user.findUnique({ where: { id } });
}

// If accidentally imported in a Client Component:
// - Exposes database credentials
// - Increases bundle size
// - Runtime errors
```

**Why this is wrong:**

- No compile-time protection
- Easy to accidentally import in client code
- Security vulnerabilities
- Larger client bundles

### Correct

```tsx
// lib/database.ts
// Protected with server-only

import "server-only";
import { db } from "./db-connection";

export async function getUser(id: string) {
  return await db.user.findUnique({ where: { id } });
}

// Now if imported in a Client Component:
// - Build error at compile time
// - Prevents security issues
// - Clear error message
```

**Why this is better:**

- Compile-time protection
- Prevents accidental client-side imports
- Protects sensitive code
- Clear error messages

### Additional Context

**Installation:**

```bash
npm install server-only
```

**Use in:**

- Database utilities
- API clients with secrets
- Server-side authentication logic
- Any code with sensitive data

**Opposite: client-only**

```tsx
import "client-only";

export function useLocalStorage() {
  // Browser-only code
}
```

### References

- [Next.js server-only](https://nextjs.org/docs/app/guides/data-security#preventing-client-side-execution-of-server-only-code)
