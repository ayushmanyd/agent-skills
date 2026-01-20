# perf-dynamic-import - Use next/dynamic for Code Splitting

**Priority:** HIGH  
**Category:** Performance & Bundle Optimization  
**Version:** Next.js 15+

### Why It Matters

Code splitting with `next/dynamic` reduces initial bundle size by loading components only when needed. This improves initial page load time and Core Web Vitals.

### Incorrect

```tsx
// app/page.tsx
// Importing heavy components statically

import { HeavyChart } from "@/components/heavy-chart";
import { VideoPlayer } from "@/components/video-player";
import { RichTextEditor } from "@/components/rich-text-editor";

export default function Page() {
  const [showChart, setShowChart] = useState(false);

  return (
    <div>
      <h1>Dashboard</h1>

      <button onClick={() => setShowChart(true)}>Show Chart</button>

      {/* All components loaded upfront, even if not visible */}
      {showChart && <HeavyChart />}
      <VideoPlayer />
      <RichTextEditor />
    </div>
  );
}
```

**Why this is wrong:**

- All component code loaded in initial bundle
- Increases Time to Interactive (TTI)
- Poor Core Web Vitals scores
- Wastes bandwidth for unused features

### Correct

```tsx
// app/page.tsx
"use client";

import { useState } from "react";
import dynamic from "next/dynamic";

// Dynamic imports with loading states
const HeavyChart = dynamic(() => import("@/components/heavy-chart"), {
  loading: () => <div>Loading chart...</div>,
  ssr: false, // Don't render on server if not needed
});

const VideoPlayer = dynamic(() => import("@/components/video-player"), {
  loading: () => <div>Loading player...</div>,
});

const RichTextEditor = dynamic(() => import("@/components/rich-text-editor"), {
  loading: () => <div>Loading editor...</div>,
});

export default function Page() {
  const [showChart, setShowChart] = useState(false);

  return (
    <div>
      <h1>Dashboard</h1>

      <button onClick={() => setShowChart(true)}>Show Chart</button>

      {/* Components loaded only when rendered */}
      {showChart && <HeavyChart />}
      <VideoPlayer />
      <RichTextEditor />
    </div>
  );
}
```

**Why this is better:**

- Smaller initial bundle size
- Faster Time to Interactive
- Components loaded on-demand
- Better Core Web Vitals

### Additional Context

**Advanced patterns:**

**Named exports:**

```tsx
const MyComponent = dynamic(() =>
  import("@/components/my-component").then((mod) => mod.MyComponent),
);
```

**Preload on hover:**

```tsx
"use client";
import dynamic from "next/dynamic";

const HeavyModal = dynamic(() => import("@/components/heavy-modal"));

export function TriggerButton() {
  const preload = () => {
    // Preload component on hover
    import("@/components/heavy-modal");
  };

  return <button onMouseEnter={preload}>Open Modal</button>;
}
```

**Disable SSR for client-only components:**

```tsx
const ClientOnlyComponent = dynamic(() => import("@/components/client-only"), {
  ssr: false,
});
```

**When to use dynamic imports:**

- Heavy third-party libraries (charts, editors, etc.)
- Components below the fold
- Modal dialogs and overlays
- Admin-only features
- Conditional features based on user permissions

**When NOT to use:**

- Critical above-the-fold content
- Small components (overhead not worth it)
- Components always needed on page load

### References

- [Next.js Dynamic Imports](https://nextjs.org/docs/app/guides/lazy-loading)
- [Code Splitting](https://nextjs.org/docs/app/guides/package-bundling)
