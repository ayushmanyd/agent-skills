# data-async-apis - Await Async Request APIs

**Priority:** CRITICAL  
**Category:** Data Fetching & Request Optimization  
**Version:** Next.js 15+

### Why It Matters

In Next.js 15+, request APIs like `cookies()`, `headers()`, `params`, and `searchParams` are asynchronous and must be awaited. This enables better pre-rendering and streaming capabilities.

### Incorrect

```tsx
// app/dashboard/page.tsx
import { cookies } from "next/headers";

// Next.js 15+: Not awaiting async APIs
export default function Dashboard({ params, searchParams }) {
  const cookieStore = cookies(); // Error in Next.js 15+
  const theme = cookieStore.get("theme");
  const userId = params.id; // Error in Next.js 15+
  const filter = searchParams.filter; // Error in Next.js 15+

  return <div>Dashboard for {userId}</div>;
}
```

**Why this is wrong:**

- Runtime errors in Next.js 15+
- Breaks pre-rendering optimizations
- Prevents proper streaming
- Not compatible with async Server Components

### Correct

```tsx
// app/dashboard/[id]/page.tsx
import { cookies, headers } from "next/headers";

// Properly awaiting async request APIs
export default async function Dashboard({
  params,
  searchParams,
}: {
  params: Promise<{ id: string }>;
  searchParams: Promise<{ filter?: string }>;
}) {
  // Await all async APIs
  const cookieStore = await cookies();
  const headersList = await headers();
  const { id } = await params;
  const { filter } = await searchParams;

  const theme = cookieStore.get("theme");
  const userAgent = headersList.get("user-agent");

  return (
    <div>
      <h1>Dashboard for {id}</h1>
      {filter && <p>Filter: {filter}</p>}
      <p>Theme: {theme?.value}</p>
    </div>
  );
}
```

**Why this is better:**

- Works correctly in Next.js 15+
- Enables proper pre-rendering
- Supports streaming
- Type-safe with proper TypeScript types

### Additional Context

**Async request APIs in Next.js 15+:**

- `cookies()` - Returns Promise<ReadonlyRequestCookies>
- `headers()` - Returns Promise<ReadonlyHeaders>
- `params` - Prop is Promise<{ [key: string]: string }>
- `searchParams` - Prop is Promise<{ [key: string]: string | string[] }>
- `draftMode()` - Returns Promise<DraftMode>

**Migration tip:**
Use the `@next/codemod` CLI to automatically update your code:

```bash
npx @next/codemod@latest next-async-request-api .
```

**Parallel awaiting:**

```tsx
// Fetch multiple async APIs in parallel
const [cookieStore, headersList, resolvedParams] = await Promise.all([
  cookies(),
  headers(),
  params,
]);
```

### References

- [Next.js 15 Async Request APIs](https://nextjs.org/docs/app/guides/upgrading/version-15#async-request-apis-breaking-change)
- [Next.js Functions Reference](https://nextjs.org/docs/app/api-reference/functions)
