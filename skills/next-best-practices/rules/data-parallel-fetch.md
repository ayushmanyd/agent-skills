# data-parallel-fetch - Parallelize Independent Data Fetches

**Priority:** CRITICAL  
**Category:** Data Fetching & Request Optimization  
**Version:** Next.js 15+

### Why It Matters

Sequential data fetching creates waterfalls that significantly slow down page loads. Parallelizing independent fetches reduces total loading time and improves user experience.

### Incorrect

```tsx
// app/dashboard/page.tsx
// Sequential fetching - creates waterfall

async function Dashboard() {
  // Each await blocks the next fetch
  const user = await fetch("/api/user").then((r) => r.json());
  const posts = await fetch("/api/posts").then((r) => r.json());
  const comments = await fetch("/api/comments").then((r) => r.json());

  // Total time: time(user) + time(posts) + time(comments)

  return (
    <div>
      <UserProfile user={user} />
      <PostList posts={posts} />
      <CommentList comments={comments} />
    </div>
  );
}
```

**Why this is wrong:**

- Creates a request waterfall
- Each request waits for the previous to complete
- Total time is the sum of all requests
- Poor user experience with slow page loads

### Correct

```tsx
// app/dashboard/page.tsx
// Parallel fetching - all requests start simultaneously

async function Dashboard() {
  // Start all fetches in parallel
  const userPromise = fetch("/api/user").then((r) => r.json());
  const postsPromise = fetch("/api/posts").then((r) => r.json());
  const commentsPromise = fetch("/api/comments").then((r) => r.json());

  // Wait for all to complete
  const [user, posts, comments] = await Promise.all([
    userPromise,
    postsPromise,
    commentsPromise,
  ]);

  // Total time: max(time(user), time(posts), time(comments))

  return (
    <div>
      <UserProfile user={user} />
      <PostList posts={posts} />
      <CommentList comments={comments} />
    </div>
  );
}

export default Dashboard;
```

**Why this is better:**

- All requests start simultaneously
- Total time is the slowest request, not the sum
- Significantly faster page loads
- Better user experience

### Additional Context

**Pattern variations:**

**Option 1: Promise.all (recommended)**

```tsx
const [data1, data2, data3] = await Promise.all([
  fetchData1(),
  fetchData2(),
  fetchData3(),
]);
```

**Option 2: Separate awaits (same effect)**

```tsx
const promise1 = fetchData1();
const promise2 = fetchData2();
const promise3 = fetchData3();

const data1 = await promise1;
const data2 = await promise2;
const data3 = await promise3;
```

**Option 3: Component-level parallelization with Suspense**

```tsx
// app/dashboard/page.tsx
export default function Dashboard() {
  return (
    <div>
      <Suspense fallback={<UserSkeleton />}>
        <UserProfile /> {/* Fetches independently */}
      </Suspense>
      <Suspense fallback={<PostsSkeleton />}>
        <PostList /> {/* Fetches independently */}
      </Suspense>
      <Suspense fallback={<CommentsSkeleton />}>
        <CommentList /> {/* Fetches independently */}
      </Suspense>
    </div>
  );
}
```

**When to use each:**

- Use Promise.all when you need all data before rendering
- Use Suspense when you want to stream content as it loads

### References

- [Next.js Data Fetching Patterns](https://nextjs.org/docs/app/getting-started/fetching-data)
- [React Suspense](https://react.dev/reference/react/Suspense)
