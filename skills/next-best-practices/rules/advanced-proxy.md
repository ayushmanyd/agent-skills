# advanced-proxy - Use proxy.ts Instead of middleware.ts

**Priority:** LOW-MEDIUM  
**Category:** Advanced Patterns & Turbopack  
**Version:** Next.js 16+

### Why It Matters

Next.js 16 replaces `middleware.ts` with `proxy.ts` to clarify network boundaries and improve request handling. This is a breaking change that requires migration.

### Incorrect

```tsx
// middleware.ts
// Deprecated in Next.js 16

import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  // Authentication check
  const token = request.cookies.get("auth-token");

  if (!token && request.nextUrl.pathname.startsWith("/dashboard")) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  return NextResponse.next();
}

export const config = {
  matcher: "/dashboard/:path*",
};
```

**Why this is wrong:**

- `middleware.ts` is deprecated in Next.js 16
- Will cause errors or warnings
- Not aligned with new architecture
- Missing new proxy.ts features

### Correct

```tsx
// proxy.ts
// New pattern in Next.js 16

import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export default function proxy(request: NextRequest) {
  // Authentication check
  const token = request.cookies.get("auth-token");

  if (!token && request.nextUrl.pathname.startsWith("/dashboard")) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  // Add custom headers
  const response = NextResponse.next();
  response.headers.set("x-custom-header", "value");

  return response;
}

export const config = {
  matcher: "/dashboard/:path*",
};
```

**Why this is better:**

- Aligned with Next.js 16 architecture
- Clearer network boundary semantics
- Future-proof
- Better integration with new features

### Additional Context

**Migration steps:**

1. Rename `middleware.ts` to `proxy.ts`
2. Update function name from `middleware` to `proxy` (or use default export)
3. Test all proxy logic
4. Update any documentation

**Common proxy patterns:**

**Authentication:**

```tsx
export default function proxy(request: NextRequest) {
  const isAuthenticated = request.cookies.has("session");

  if (!isAuthenticated && isProtectedRoute(request.nextUrl.pathname)) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  return NextResponse.next();
}
```

**Geolocation redirect:**

```tsx
export default function proxy(request: NextRequest) {
  const country = request.geo?.country || "US";
  const response = NextResponse.next();

  response.cookies.set("user-country", country);

  return response;
}
```

**A/B testing:**

```tsx
export default function proxy(request: NextRequest) {
  const bucket = Math.random() < 0.5 ? "A" : "B";
  const response = NextResponse.next();

  response.cookies.set("ab-test-bucket", bucket);

  return response;
}
```

**Rate limiting:**

```tsx
import { Ratelimit } from "@upstash/ratelimit";
import { Redis } from "@upstash/redis";

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(10, "10 s"),
});

export default async function proxy(request: NextRequest) {
  const ip = request.ip ?? "127.0.0.1";
  const { success } = await ratelimit.limit(ip);

  if (!success) {
    return new NextResponse("Too Many Requests", { status: 429 });
  }

  return NextResponse.next();
}
```

**Matcher configuration:**

```tsx
export const config = {
  matcher: [
    // Match all paths except static files
    "/((?!_next/static|_next/image|favicon.ico).*)",
    // Or specific paths
    "/dashboard/:path*",
    "/api/:path*",
  ],
};
```

### References

- [Next.js 16 Proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)
- [Next.js 16 Migration Guide](https://nextjs.org/docs/app/guides/upgrading/version-16)
