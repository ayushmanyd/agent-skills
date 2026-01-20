# Next.js Best Practices

Comprehensive best practices for Next.js 15 & 16 applications, covering App Router, Server Components, Cache Components, React Compiler, and performance optimization.

## Overview

This skill provides production-ready guidance for building modern Next.js applications. It covers both Next.js 15 and Next.js 16, with clear version-specific annotations for breaking changes and new features.

## When to Use

Use this skill when:
- **Writing new Next.js applications** - Follow best practices from the start
- **Migrating to Next.js 15/16** - Understand breaking changes and new patterns
- **Optimizing performance** - Apply proven optimization techniques
- **Reviewing code** - Ensure adherence to Next.js best practices
- **Implementing data fetching** - Choose the right caching and fetching strategies

## Categories Covered

### Critical Priority
1. **App Router & Server Components** - Proper use of Server and Client Components
2. **Cache Components (Next.js 16)** - Explicit caching with "use cache" directive
3. **Data Fetching** - Async request APIs, parallel fetching, deduplication

### High Priority
4. **React Compiler (Next.js 16)** - Automatic memoization and optimization
5. **Performance & Bundle** - Code splitting, tree shaking, Turbopack

### Medium-High Priority
6. **Routing & Navigation** - Loading states, error boundaries, parallel routes

### Medium Priority
7. **Image & Asset Optimization** - next/image, modern formats, responsive images
8. **SEO & Metadata** - Metadata API, sitemaps, Open Graph
9. **Error Handling** - Error boundaries, loading states, Suspense

### Low-Medium Priority
10. **Advanced Patterns** - proxy.ts, Server Actions, instrumentation

## Key Features

### Next.js 15 Support
- React 19 integration
- Async request APIs (cookies, headers, params)
- Turbopack Dev (stable)
- Uncached by default behavior
- Enhanced forms with next/form

### Next.js 16 Support
- Cache Components with "use cache"
- React Compiler (stable)
- Turbopack for production (default)
- proxy.ts replaces middleware.ts
- Enhanced caching APIs (updateTag, cacheLife)
- Layout deduplication
- Incremental prefetching

## Installation

### For Claude Code / Cursor
```bash
cp -r skills/next-best-practices ~/.claude/skills/
```

### For claude.ai
Upload the skill to project knowledge or paste SKILL.md contents into the conversation.

## Usage

The skill is automatically activated when working with Next.js code. AI agents will reference these guidelines when:
- Creating new pages or components
- Implementing routing patterns
- Setting up data fetching
- Optimizing performance
- Reviewing or refactoring code

## Rule Structure

Each rule includes:
- **Priority level** - Impact assessment (Critical, High, Medium, Low)
- **Version compatibility** - Which Next.js versions it applies to
- **Problem explanation** - Why the pattern matters
- **Incorrect example** - Common mistakes to avoid
- **Correct example** - Recommended implementation
- **Additional context** - References and related patterns

## Migration Guidance

### Upgrading to Next.js 15
1. Update Node.js to 18.18.0+
2. Make request APIs async (await cookies(), headers(), params)
3. Opt-in to caching where needed (fetch with cache option)
4. Update next/image usage (sharp is now default)

### Upgrading to Next.js 16
1. Update Node.js to 20.9+
2. Migrate middleware.ts to proxy.ts
3. Adopt "use cache" for explicit caching
4. Enable React Compiler in next.config.js
5. Remove manual useMemo/useCallback where compiler handles it
6. Add default.js to all parallel route slots

## Contributing

This skill is maintained as part of the agent-skills repository. Contributions are welcome to keep best practices current with the latest Next.js releases.

## License

MIT
