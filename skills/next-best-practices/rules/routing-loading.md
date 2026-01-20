# routing-loading - Create loading.tsx for Route Segments

**Priority:** MEDIUM-HIGH  
**Category:** Routing & Navigation  
**Version:** Next.js 15+

### Why It Matters

The `loading.tsx` file provides instant loading UI while route segments load, improving perceived performance and user experience through streaming.

### Incorrect

```tsx
// app/dashboard/page.tsx
// No loading state - users see blank screen while loading

async function DashboardPage() {
  const data = await fetchDashboardData(); // Blocks entire page

  return (
    <div>
      <h1>Dashboard</h1>
      <DataDisplay data={data} />
    </div>
  );
}

export default DashboardPage;
```

**Why this is wrong:**

- Users see blank screen while data loads
- No feedback that something is happening
- Poor perceived performance
- No streaming benefits

### Correct

```tsx
// app/dashboard/loading.tsx
// Instant loading UI

export default function Loading() {
  return (
    <div>
      <h1>Dashboard</h1>
      <div className="animate-pulse">
        <div className="h-8 bg-gray-200 rounded w-1/4 mb-4"></div>
        <div className="h-64 bg-gray-200 rounded"></div>
      </div>
    </div>
  );
}

// app/dashboard/page.tsx
async function DashboardPage() {
  const data = await fetchDashboardData();

  return (
    <div>
      <h1>Dashboard</h1>
      <DataDisplay data={data} />
    </div>
  );
}

export default DashboardPage;
```

**Why this is better:**

- Instant loading UI shown immediately
- Users know content is loading
- Better perceived performance
- Enables streaming with Suspense

### Additional Context

**How it works:**

- `loading.tsx` automatically wraps page in `<Suspense>`
- Shows loading UI while page/layout loads
- Works for entire route segment

**Nested loading states:**

```
app/
  dashboard/
    loading.tsx          # Shown while dashboard loads
    page.tsx
    settings/
      loading.tsx        # Shown while settings loads
      page.tsx
```

**Skeleton patterns:**

```tsx
// loading.tsx with realistic skeleton
export default function Loading() {
  return (
    <div className="space-y-4">
      {/* Header skeleton */}
      <div className="h-12 bg-gray-200 rounded-lg w-1/3" />

      {/* Content skeleton */}
      <div className="grid grid-cols-3 gap-4">
        {[...Array(6)].map((_, i) => (
          <div key={i} className="h-32 bg-gray-200 rounded-lg" />
        ))}
      </div>
    </div>
  );
}
```

### References

- [Next.js Loading UI](https://nextjs.org/docs/app/api-reference/file-conventions/loading)
