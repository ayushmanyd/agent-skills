# cache-use-cache - Use "use cache" for Explicit Caching

**Priority:** CRITICAL  
**Category:** Cache Components & Caching  
**Version:** Next.js 16+

### Why It Matters

Next.js 16 introduces Cache Components with the "use cache" directive, providing explicit, opt-in caching control. This replaces the implicit caching model and gives you granular control over what gets cached and for how long.

### Incorrect

```tsx
// app/products/page.tsx
// Next.js 16: No caching by default, data fetched on every request

async function ProductsPage() {
  const products = await fetch("https://api.example.com/products").then((res) =>
    res.json(),
  );

  return (
    <div>
      {products.map((product) => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}

export default ProductsPage;
```

**Why this is wrong:**

- In Next.js 16, fetch is NOT cached by default
- Data is fetched on every request, causing unnecessary load
- Slower response times for users
- Higher server costs

### Correct

```tsx
// app/products/page.tsx
import { unstable_cacheLife as cacheLife } from "next/cache";

// Explicit caching with "use cache"
async function ProductsPage() {
  "use cache";
  cacheLife("hours"); // Cache for 1 hour

  const products = await fetch("https://api.example.com/products").then((res) =>
    res.json(),
  );

  return (
    <div>
      {products.map((product) => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}

export default ProductsPage;
```

**Why this is better:**

- Explicit caching behavior - clear and predictable
- Reduces server load and API calls
- Faster response times
- Full control over cache duration

### Additional Context

**Cache profiles available:**

- `cacheLife('seconds')` - 1 second
- `cacheLife('minutes')` - 5 minutes
- `cacheLife('hours')` - 1 hour
- `cacheLife('days')` - 7 days
- `cacheLife('weeks')` - 30 days
- `cacheLife('max')` - 1 year

**Custom cache profiles:**

```tsx
cacheLife({
  stale: 60, // Serve stale for 60 seconds
  revalidate: 300, // Revalidate after 5 minutes
  expire: 3600, // Expire after 1 hour
});
```

**Where to use "use cache":**

- Component functions
- Utility functions
- Data fetching functions
- Route handlers

### References

- [Next.js Cache Components](https://nextjs.org/docs/app/api-reference/directives/use-cache)
- [Next.js 16 Caching](https://nextjs.org/docs/app/guides/caching)
