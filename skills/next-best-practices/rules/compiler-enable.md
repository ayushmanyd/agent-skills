# compiler-enable - Enable React Compiler

**Priority:** HIGH  
**Category:** React Compiler  
**Version:** Next.js 16+

### Why It Matters

The React Compiler (stable in Next.js 16) automatically optimizes your components by applying memoization during the build process. This eliminates the need for manual `useMemo`, `useCallback`, and `React.memo` calls.

### Incorrect

```tsx
// next.config.js
// Not enabling the React Compiler

/** @type {import('next').NextConfig} */
const nextConfig = {
  // Missing compiler configuration
};

module.exports = nextConfig;

// Component with manual memoization
("use client");
import { useMemo, useCallback } from "react";

export function ProductList({ products, onSelect }) {
  // Manual memoization needed without compiler
  const sortedProducts = useMemo(() => {
    return products.sort((a, b) => a.name.localeCompare(b.name));
  }, [products]);

  const handleClick = useCallback(
    (id) => {
      onSelect(id);
    },
    [onSelect],
  );

  return (
    <div>
      {sortedProducts.map((product) => (
        <div key={product.id} onClick={() => handleClick(product.id)}>
          {product.name}
        </div>
      ))}
    </div>
  );
}
```

**Why this is wrong:**

- Missing automatic optimization benefits
- Requires manual memoization everywhere
- More boilerplate code
- Easy to forget memoization in critical paths

### Correct

```tsx
// next.config.js
// Enable React Compiler

/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    reactCompiler: true,
  },
};

module.exports = nextConfig;

// Component without manual memoization - compiler handles it!
("use client");

export function ProductList({ products, onSelect }) {
  // No useMemo needed - compiler optimizes automatically
  const sortedProducts = products.sort((a, b) => a.name.localeCompare(b.name));

  // No useCallback needed - compiler optimizes automatically
  const handleClick = (id) => {
    onSelect(id);
  };

  return (
    <div>
      {sortedProducts.map((product) => (
        <div key={product.id} onClick={() => handleClick(product.id)}>
          {product.name}
        </div>
      ))}
    </div>
  );
}
```

**Why this is better:**

- Automatic optimization without manual work
- Cleaner, more readable code
- Compiler applies memoization optimally
- Prevents unnecessary re-renders automatically

### Additional Context

**Configuration options:**

```tsx
// next.config.js
const nextConfig = {
  experimental: {
    reactCompiler: {
      compilationMode: "annotation", // Only compile annotated components
      // or 'all' for all components (default)
    },
  },
};
```

**Annotation mode:**

```tsx
"use memo"; // Opt-in to compiler optimization for this component
export function MyComponent() {
  // ...
}
```

**What the compiler optimizes:**

- Component re-renders
- Hook dependencies
- Event handler stability
- Computed values

**Requirements:**

- Pure components (no side effects in render)
- Stable props structure
- Follow React Rules of Hooks

### References

- [React Compiler Documentation](https://react.dev/learn/react-compiler)
- [Next.js React Compiler](https://nextjs.org/docs/app/api-reference/config/next-config-js/reactCompiler)
