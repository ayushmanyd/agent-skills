# app-server-default - Use Server Components by Default

**Priority:** CRITICAL  
**Category:** App Router & Server Components  
**Version:** Next.js 15+

### Why It Matters

In the App Router, all components are Server Components by default. Server Components run on the server, reducing client-side JavaScript, improving initial page load, and allowing direct access to backend resources like databases and APIs.

### Incorrect

```tsx
// app/dashboard/page.tsx
"use client"; // Unnecessary - makes entire page client-side

export default function Dashboard() {
  return (
    <div>
      <Header />
      <Stats />
      <UserList />
    </div>
  );
}
```

**Why this is wrong:**

- Adds unnecessary "use client" directive when not needed
- Forces all child components to be client-side
- Increases JavaScript bundle size
- Prevents server-side data fetching optimizations

### Correct

```tsx
// app/dashboard/page.tsx
// Server Component by default (no directive needed)

async function Dashboard() {
  // Can fetch data directly on the server
  const stats = await getStats();
  const users = await getUsers();

  return (
    <div>
      <Header />
      <Stats data={stats} />
      <UserList users={users} />
    </div>
  );
}

export default Dashboard;
```

**Why this is better:**

- No client-side JavaScript for this component
- Can fetch data directly without API routes
- Faster initial page load
- Better SEO (content rendered on server)
- Keeps sensitive logic on the server

### Additional Context

Only add "use client" when you need:

- Event handlers (onClick, onChange, etc.)
- React hooks (useState, useEffect, etc.)
- Browser APIs (window, localStorage, etc.)
- Third-party libraries that use client-only features

### References

- [Next.js Server Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)
- [React Server Components](https://react.dev/reference/rsc/server-components)
