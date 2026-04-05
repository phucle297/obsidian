# Next.js Image (`next/image`)

## Overview

- Extends HTML `<img>` element
- Import: `import Image from 'next/image'`

## Key Benefits

- **Size optimization**: auto-serve correct size per device, modern formats (WebP)
- **Visual stability**: prevents layout shift (CLS) during loading
- **Lazy loading**: loads images only when entering viewport (native browser) + blur-up placeholder
- **Asset flexibility**: on-demand resize, even for remote images

## Local Images

- Store in `public/` folder at project root
- **String path**: `src="/profile.png"` — must provide `width` and `height`
- **Static import** (recommended): `import ProfileImage from './profile.png'`
    - Auto-detects `width`, `height`
    - Auto-generates `blurDataURL`
    - Supports `placeholder="blur"`

## Remote Images

- Pass URL string to `src`: `src="https://..."`
- **Required**: `width`, `height` (or use `fill`)
- Optional: `blurDataURL`
- **Must configure `remotePatterns`** in `next.config.js` to whitelist domains
    - Fields: `protocol`, `hostname`, `port`, `pathname`, `search`
    - Be as specific as possible (security)

## Remote Patterns Config

```ts
// next.config.ts
images: {
  remotePatterns: [
    {
      protocol: 'https',
      hostname: 's3.amazonaws.com',
      pathname: '/my-bucket/**',
    },
  ],
}
```

## Important Props

- `src` — local path or remote URL
- `alt` — image description (accessibility)
- `width` / `height` — dimensions (prevents CLS)
- `fill` — fill parent container (replaces width/height)
- `placeholder` — `"blur"` | `"empty"`
- `blurDataURL` — base64 string for blur placeholder
- `priority` — preload image (use for LCP image)

## Notes

- `fill` requires parent to have `position: relative`
- Use `priority` for above-the-fold (LCP) images
- Remote images need domain whitelist → security