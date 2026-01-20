# error-boundaries - Implement error.tsx for Error Handling

**Priority:** MEDIUM  
**Category:** Error Handling & Loading States  
**Version:** Next.js 15+

### Why It Matters

Error boundaries with `error.tsx` provide graceful error handling, allowing users to recover from errors without losing their entire session.

### Incorrect

```tsx
// app/dashboard/page.tsx
// No error handling - errors crash the entire app

async function DashboardPage() {
  const data = await fetchData(); // If this fails, app crashes

  return <DataDisplay data={data} />;
}

export default DashboardPage;
```

**Why this is wrong:**

- Unhandled errors crash the entire application
- No user feedback when errors occur
- Poor user experience
- No recovery mechanism

### Correct

```tsx
// app/dashboard/error.tsx
// Error boundary for graceful handling

"use client"; // Error components must be Client Components

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div>
      <h2>Something went wrong!</h2>
      <p>{error.message}</p>
      <button onClick={() => reset()}>Try again</button>
    </div>
  );
}

// app/dashboard/page.tsx
async function DashboardPage() {
  const data = await fetchData(); // Errors caught by error.tsx

  return <DataDisplay data={data} />;
}

export default DashboardPage;
```

**Why this is better:**

- Errors are caught and handled gracefully
- Users can retry without losing context
- Better user experience
- Isolated error handling per route segment

### Additional Context

**Error boundary hierarchy:**

```
app/
  error.tsx              # Catches errors in all routes
  dashboard/
    error.tsx            # Catches errors in dashboard
    page.tsx
    settings/
      error.tsx          # Catches errors in settings
      page.tsx
```

**Global error handler:**

```tsx
// app/global-error.tsx
"use client";

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <html>
      <body>
        <h2>Something went wrong!</h2>
        <button onClick={() => reset()}>Try again</button>
      </body>
    </html>
  );
}
```

### References

- [Next.js Error Handling](https://nextjs.org/docs/app/getting-started/error-handling)
