# image-next-image - Use next/image for Optimization

**Priority:** MEDIUM  
**Category:** Image & Asset Optimization  
**Version:** Next.js 15+

### Why It Matters

The `next/image` component automatically optimizes images, serving modern formats (WebP, AVIF), responsive sizes, and lazy loading. This significantly improves page load performance and Core Web Vitals.

### Incorrect

```tsx
// Using regular img tag

export function ProductCard({ product }) {
  return (
    <div>
      <img src={product.imageUrl} alt={product.name} width="500" height="500" />
      <h3>{product.name}</h3>
    </div>
  );
}
```

**Why this is wrong:**

- No automatic format optimization (stuck with original format)
- No responsive image sizing
- No lazy loading by default
- No blur placeholder for better UX
- Larger file sizes
- Poor Largest Contentful Paint (LCP)

### Correct

```tsx
// Using next/image with optimization

import Image from "next/image";

export function ProductCard({ product }) {
  return (
    <div>
      <Image
        src={product.imageUrl}
        alt={product.name}
        width={500}
        height={500}
        sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
        placeholder="blur"
        blurDataURL={product.blurDataUrl}
        priority={false} // Set true for above-fold images
      />
      <h3>{product.name}</h3>
    </div>
  );
}
```

**Why this is better:**

- Automatic WebP/AVIF conversion
- Responsive image sizing
- Automatic lazy loading
- Blur placeholder for better UX
- Optimized file sizes
- Better LCP scores

### Additional Context

**Priority images (above the fold):**

```tsx
<Image
  src="/hero.jpg"
  alt="Hero image"
  width={1200}
  height={600}
  priority // Disable lazy loading, preload image
  sizes="100vw"
/>
```

**Fill container (unknown dimensions):**

```tsx
<div style={{ position: "relative", width: "100%", height: "400px" }}>
  <Image
    src="/background.jpg"
    alt="Background"
    fill
    style={{ objectFit: "cover" }}
    sizes="100vw"
  />
</div>
```

**External images (configure in next.config.js):**

```tsx
// next.config.js
module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: "https",
        hostname: "example.com",
        pathname: "/images/**",
      },
    ],
  },
};
```

**Blur placeholder generation:**

```tsx
import { getPlaiceholder } from "plaiceholder";

async function getBase64(imageUrl: string) {
  const res = await fetch(imageUrl);
  const buffer = await res.arrayBuffer();
  const { base64 } = await getPlaiceholder(Buffer.from(buffer));
  return base64;
}
```

**Sizes prop guidance:**

- Mobile: `100vw` (full width)
- Tablet: `50vw` (half width)
- Desktop: `33vw` (third width)
- Adjust based on your layout

**Configuration options:**

```tsx
// next.config.js
module.exports = {
  images: {
    formats: ["image/avif", "image/webp"], // Preferred formats
    deviceSizes: [640, 750, 828, 1080, 1200, 1920, 2048, 3840],
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384],
  },
};
```

### References

- [Next.js Image Optimization](https://nextjs.org/docs/app/getting-started/images)
- [next/image API Reference](https://nextjs.org/docs/app/api-reference/components/image)
