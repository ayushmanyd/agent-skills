# seo-metadata-api - Use Metadata API for SEO

**Priority:** MEDIUM  
**Category:** SEO & Metadata  
**Version:** Next.js 15+

### Why It Matters

The Metadata API provides a type-safe way to define SEO metadata, ensuring proper tags for search engines and social media platforms.

### Incorrect

```tsx
// app/page.tsx
// Using Head component (Pages Router pattern)

import Head from "next/head";

export default function Page() {
  return (
    <>
      <Head>
        <title>My Page</title>
        <meta name="description" content="Page description" />
      </Head>
      <div>Content</div>
    </>
  );
}
```

**Why this is wrong:**

- `next/head` doesn't work in App Router
- Not type-safe
- Harder to maintain
- Missing modern metadata features

### Correct

```tsx
// app/page.tsx
// Using Metadata API

import { Metadata } from "next";

export const metadata: Metadata = {
  title: "My Page",
  description: "Page description",
  keywords: ["nextjs", "react", "web development"],
  authors: [{ name: "Your Name" }],
  openGraph: {
    title: "My Page",
    description: "Page description",
    url: "https://example.com",
    siteName: "My Site",
    images: [
      {
        url: "https://example.com/og-image.jpg",
        width: 1200,
        height: 630,
        alt: "My Page OG Image",
      },
    ],
    locale: "en_US",
    type: "website",
  },
  twitter: {
    card: "summary_large_image",
    title: "My Page",
    description: "Page description",
    images: ["https://example.com/twitter-image.jpg"],
  },
  robots: {
    index: true,
    follow: true,
    googleBot: {
      index: true,
      follow: true,
      "max-video-preview": -1,
      "max-image-preview": "large",
      "max-snippet": -1,
    },
  },
};

export default function Page() {
  return <div>Content</div>;
}
```

**Why this is better:**

- Type-safe metadata
- Works in App Router
- Comprehensive SEO coverage
- Automatic deduplication

### Additional Context

**Dynamic metadata:**

```tsx
// app/blog/[slug]/page.tsx
import { Metadata } from "next";

export async function generateMetadata({
  params,
}: {
  params: Promise<{ slug: string }>;
}): Promise<Metadata> {
  const { slug } = await params;
  const post = await getPost(slug);

  return {
    title: post.title,
    description: post.excerpt,
    openGraph: {
      title: post.title,
      description: post.excerpt,
      images: [post.coverImage],
    },
  };
}
```

**Template metadata (layout):**

```tsx
// app/layout.tsx
export const metadata: Metadata = {
  title: {
    template: "%s | My Site",
    default: "My Site",
  },
  description: "Default description",
};
```

**Viewport and theme color:**

```tsx
export const metadata: Metadata = {
  viewport: {
    width: "device-width",
    initialScale: 1,
    maximumScale: 1,
  },
  themeColor: [
    { media: "(prefers-color-scheme: light)", color: "#ffffff" },
    { media: "(prefers-color-scheme: dark)", color: "#000000" },
  ],
};
```

### References

- [Next.js Metadata API](https://nextjs.org/docs/app/getting-started/metadata-and-og-images)
- [Metadata Reference](https://nextjs.org/docs/app/api-reference/functions/generate-metadata)
