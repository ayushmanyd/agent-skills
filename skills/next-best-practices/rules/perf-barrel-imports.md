# perf-barrel-imports - Avoid Barrel File Imports

**Priority:** HIGH  
**Category:** Performance & Bundle Optimization  
**Version:** Next.js 15+

### Why It Matters

Barrel files (index.ts that re-export everything) can significantly increase bundle size by importing unused code. Direct imports enable better tree-shaking.

### Incorrect

```tsx
// Importing from barrel file

import { Button, Card, Modal } from "@/components";
// This might import ALL components from the barrel file
```

**Why this is wrong:**

- May import entire component library
- Poor tree-shaking
- Larger bundle size
- Slower build times

### Correct

```tsx
// Direct imports

import { Button } from "@/components/button";
import { Card } from "@/components/card";
import { Modal } from "@/components/modal";
```

**Why this is better:**

- Only imports what you need
- Better tree-shaking
- Smaller bundle size
- Faster builds

### Additional Context

**Especially important for:**

- Icon libraries (lucide-react, react-icons)
- UI component libraries
- Utility libraries (lodash, date-fns)

**Example with icons:**

```tsx
// Bad
import { IconName } from "react-icons/all";

// Good
import { IconName } from "react-icons/fa";
```

### References

- [Next.js Package Import Optimization](https://nextjs.org/docs/app/building-your-application/optimizing/package-bundling)
