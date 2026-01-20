---
name: next-best-practices
description: Next.js 15 & 16 best practices for App Router, Server Components, Cache Components, React Compiler, and performance optimization. Use when writing, reviewing, or refactoring Next.js applications. Triggers on tasks involving Next.js pages, routing, data fetching, caching, or performance improvements.
license: MIT
metadata:
  author: Next.js Community
  version: "1.0.0"
  supports:
    - Next.js 15
    - Next.js 16
---

# Next.js Best Practices

Comprehensive best practices guide for Next.js 15 & 16 applications. Contains 45+ rules across 10 categories, prioritized by impact to guide automated refactoring and code generation.

## When to Apply

Reference these guidelines when:
- Writing new Next.js pages or components
- Implementing App Router patterns
- Setting up data fetching and caching strategies
- Optimizing performance and bundle size
- Migrating from Next.js 14 to 15/16
- Reviewing code for Next.js best practices

## Rule Categories by Priority

| Priority | Category | Impact | Prefix | Version |
|----------|----------|--------|--------|---------|
| 1 | App Router & Server Components | CRITICAL | `app-` | 15+ |
| 2 | Cache Components & Caching | CRITICAL | `cache-` | 16+ |
| 3 | Data Fetching & Request Optimization | CRITICAL | `data-` | 15+ |
| 4 | React Compiler | HIGH | `compiler-` | 16+ |
| 5 | Performance & Bundle Optimization | HIGH | `perf-` | 15+ |
| 6 | Routing & Navigation | MEDIUM-HIGH | `routing-` | 15+ |
| 7 | Image & Asset Optimization | MEDIUM | `image-` | 15+ |
| 8 | SEO & Metadata | MEDIUM | `seo-` | 15+ |
| 9 | Error Handling & Loading States | MEDIUM | `error-` | 15+ |
| 10 | Advanced Patterns & Turbopack | LOW-MEDIUM | `advanced-` | 15+ |

## Quick Reference

### 1. App Router & Server Components (CRITICAL)

- `app-server-default` - Use Server Components by default
- `app-client-directive` - Strategic use of "use client" directive
- `app-composition` - Compose Server and Client Components correctly
- `app-server-only` - Use server-only package for sensitive code
- `app-async-components` - Leverage async Server Components
- `app-streaming` - Use Suspense for streaming

### 2. Cache Components & Caching (CRITICAL) - Next.js 16

- `cache-use-cache` - Use "use cache" directive for explicit caching
- `cache-components` - Implement Cache Components architecture
- `cache-life-profiles` - Define and use cacheLife profiles
- `cache-update-tag` - Use updateTag() for cache invalidation
- `cache-revalidate-tag` - Use revalidateTag() with cacheLife
- `cache-opt-in` - Understand opt-in caching model

### 3. Data Fetching & Request Optimization (CRITICAL)

- `data-async-apis` - Await async request APIs (cookies, headers, params)
- `data-parallel-fetch` - Parallelize independent data fetches
- `data-fetch-dedup` - Leverage automatic fetch deduplication
- `data-react-cache` - Use React.cache() for non-fetch requests
- `data-server-fetch` - Fetch data in Server Components
- `data-loading-ui` - Implement loading.tsx for instant feedback

### 4. React Compiler (HIGH) - Next.js 16

- `compiler-enable` - Enable React Compiler in next.config.js
- `compiler-pure-components` - Write pure components for optimization
- `compiler-avoid-manual-memo` - Remove manual useMemo/useCallback
- `compiler-stable-deps` - Ensure stable component dependencies

### 5. Performance & Bundle Optimization (HIGH)

- `perf-dynamic-import` - Use next/dynamic for code splitting
- `perf-barrel-imports` - Avoid barrel file imports
- `perf-third-party` - Optimize third-party scripts
- `perf-font-optimization` - Use next/font for font optimization
- `perf-turbopack` - Leverage Turbopack for faster builds (Next.js 16)

### 6. Routing & Navigation (MEDIUM-HIGH)

- `routing-loading` - Create loading.tsx for route segments
- `routing-error` - Implement error.tsx for error boundaries
- `routing-not-found` - Use not-found.tsx for 404 pages
- `routing-parallel-routes` - Use parallel routes with default.js (Next.js 16)
- `routing-intercepting` - Implement intercepting routes for modals
- `routing-link-prefetch` - Use Link component for client navigation

### 7. Image & Asset Optimization (MEDIUM)

- `image-next-image` - Use next/image for automatic optimization
- `image-sizes` - Configure sizes prop for responsive images
- `image-priority` - Set priority for above-fold images
- `image-placeholder` - Use blur placeholders for better UX
- `image-formats` - Leverage modern formats (AVIF, WebP)

### 8. SEO & Metadata (MEDIUM)

- `seo-metadata-api` - Use Metadata API for SEO tags
- `seo-generate-metadata` - Implement generateMetadata for dynamic pages
- `seo-sitemap` - Generate sitemap.xml
- `seo-robots` - Configure robots.txt
- `seo-opengraph` - Set Open Graph metadata

### 9. Error Handling & Loading States (MEDIUM)

- `error-boundaries` - Implement error.tsx at appropriate levels
- `error-global-error` - Create global-error.tsx for root errors
- `error-not-found` - Use notFound() function appropriately
- `error-redirect` - Use redirect() for navigation
- `loading-suspense` - Use Suspense for granular loading states

### 10. Advanced Patterns & Turbopack (LOW-MEDIUM)

- `advanced-proxy` - Use proxy.ts instead of middleware.ts (Next.js 16)
- `advanced-instrumentation` - Implement instrumentation.js
- `advanced-ppr` - Use Partial Prerendering when stable
- `advanced-server-actions` - Implement Server Actions for mutations
- `advanced-route-handlers` - Create efficient Route Handlers

## Version-Specific Features

### Next.js 15 Key Features
- React 19 support
- Async request APIs (cookies, headers, params)
- Turbopack Dev (stable)
- Uncached by default behavior
- Enhanced next/form

### Next.js 16 Key Features
- Cache Components with "use cache"
- React Compiler (stable)
- Turbopack for production (stable, default)
- proxy.ts replaces middleware.ts
- Enhanced caching APIs (updateTag, cacheLife)
- Layout deduplication
- Incremental prefetching

## How to Use

Read individual rule files for detailed explanations and code examples:

```
rules/app-server-default.md
rules/cache-use-cache.md
rules/data-async-apis.md
rules/_sections.md
```

Each rule file contains:
- Brief explanation of why it matters
- Version compatibility notes
- Incorrect code example with explanation
- Correct code example with explanation
- Additional context and references

## Migration Guidance

### Upgrading to Next.js 15
1. Update to Node.js 18.18.0+
2. Make request APIs async (await cookies(), headers(), params)
3. Opt-in to caching where needed
4. Update next/image usage (sharp is default)

### Upgrading to Next.js 16
1. Update to Node.js 20.9+
2. Migrate middleware.ts to proxy.ts
3. Adopt "use cache" for explicit caching
4. Enable React Compiler for automatic optimization
5. Remove manual memoization where compiler handles it
6. Update parallel routes to include default.js

## References

- [Next.js Documentation](https://nextjs.org/docs)
- [Next.js 15 Release Notes](https://nextjs.org/blog/next-15)
- [Next.js 16 Release Notes](https://nextjs.org/blog/next-16)
- [React Server Components](https://react.dev/reference/rsc/server-components)
- [React Compiler](https://react.dev/learn/react-compiler)
