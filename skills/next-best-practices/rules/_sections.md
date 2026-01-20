# Rule Categories

This document provides an overview of all rule categories and their priorities.

## 1. App Router & Server Components (CRITICAL)

Rules for properly using Server and Client Components in the App Router.

**Impact:** Incorrect usage can lead to unnecessary client-side JavaScript, security vulnerabilities, and poor performance.

**Rules:**

- app-server-default
- app-client-directive
- app-composition
- app-server-only
- app-async-components
- app-streaming

## 2. Cache Components & Caching (CRITICAL) - Next.js 16

Rules for the new explicit caching model in Next.js 16.

**Impact:** Improper caching can lead to stale data, poor performance, or excessive server load.

**Rules:**

- cache-use-cache
- cache-components
- cache-life-profiles
- cache-update-tag
- cache-revalidate-tag
- cache-opt-in

## 3. Data Fetching & Request Optimization (CRITICAL)

Rules for efficient data fetching and request handling.

**Impact:** Poor data fetching patterns cause waterfalls, slow page loads, and degraded UX.

**Rules:**

- data-async-apis
- data-parallel-fetch
- data-fetch-dedup
- data-react-cache
- data-server-fetch
- data-loading-ui

## 4. React Compiler (HIGH) - Next.js 16

Rules for leveraging the React Compiler for automatic optimization.

**Impact:** Proper compiler usage eliminates manual memoization and improves performance.

**Rules:**

- compiler-enable
- compiler-pure-components
- compiler-avoid-manual-memo
- compiler-stable-deps

## 5. Performance & Bundle Optimization (HIGH)

Rules for optimizing bundle size and runtime performance.

**Impact:** Large bundles and poor performance directly affect Core Web Vitals and user experience.

**Rules:**

- perf-dynamic-import
- perf-barrel-imports
- perf-third-party
- perf-font-optimization
- perf-turbopack

## 6. Routing & Navigation (MEDIUM-HIGH)

Rules for implementing robust routing patterns.

**Impact:** Poor routing UX leads to loading delays and error states.

**Rules:**

- routing-loading
- routing-error
- routing-not-found
- routing-parallel-routes
- routing-intercepting
- routing-link-prefetch

## 7. Image & Asset Optimization (MEDIUM)

Rules for optimizing images and static assets.

**Impact:** Unoptimized images are a major contributor to slow page loads.

**Rules:**

- image-next-image
- image-sizes
- image-priority
- image-placeholder
- image-formats

## 8. SEO & Metadata (MEDIUM)

Rules for implementing proper SEO and metadata.

**Impact:** Poor SEO affects discoverability and social sharing.

**Rules:**

- seo-metadata-api
- seo-generate-metadata
- seo-sitemap
- seo-robots
- seo-opengraph

## 9. Error Handling & Loading States (MEDIUM)

Rules for graceful error handling and loading states.

**Impact:** Poor error handling creates bad UX and makes debugging difficult.

**Rules:**

- error-boundaries
- error-global-error
- error-not-found
- error-redirect
- loading-suspense

## 10. Advanced Patterns & Turbopack (LOW-MEDIUM)

Advanced patterns and tooling optimizations.

**Impact:** These patterns provide incremental improvements for specific use cases.

**Rules:**

- advanced-proxy
- advanced-instrumentation
- advanced-ppr
- advanced-server-actions
- advanced-route-handlers
