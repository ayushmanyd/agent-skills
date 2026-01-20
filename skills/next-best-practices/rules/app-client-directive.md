# app-client-directive - Strategic Use of "use client"

**Priority:** CRITICAL  
**Category:** App Router & Server Components  
**Version:** Next.js 15+

### Why It Matters

The "use client" directive marks the boundary between Server and Client Components. Using it strategically minimizes client-side JavaScript while maintaining interactivity where needed.

### Incorrect

```tsx
// app/page.tsx
"use client"; // Too high in the tree

import { Sidebar } from "./sidebar";
import { Content } from "./content";
import { Footer } from "./footer";

export default function Page() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <Sidebar /> {/* Now client-side unnecessarily */}
      <Content /> {/* Now client-side unnecessarily */}
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <Footer /> {/* Now client-side unnecessarily */}
    </div>
  );
}
```

**Why this is wrong:**

- Makes entire page and all children client-side
- Increases bundle size significantly
- Prevents server-side data fetching in child components
- Loses SEO benefits for static content

### Correct

```tsx
// app/page.tsx
// Server Component (default)

import { Sidebar } from "./sidebar"; // Server Component
import { Content } from "./content"; // Server Component
import { Counter } from "./counter"; // Client Component
import { Footer } from "./footer"; // Server Component

export default function Page() {
  return (
    <div>
      <Sidebar />
      <Content />
      <Counter /> {/* Only this is client-side */}
      <Footer />
    </div>
  );
}

// components/counter.tsx
("use client"); // Only where needed

import { useState } from "react";

export function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

**Why this is better:**

- Minimizes client-side JavaScript
- Only interactive components are client-side
- Server Components can still fetch data
- Better performance and SEO

### Additional Context

**Best practices:**

- Push "use client" as deep as possible in the component tree
- Extract interactive parts into separate Client Components
- Keep data fetching in Server Components
- Compose Server and Client Components together

**The "use client" boundary:**

- Marks the entry point to client-side code
- All imports in a "use client" file become client-side
- Children can be Server Components if passed as props

### References

- [Next.js Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)
