# Next.js Metadata and OG Images

## Overview

- Metadata APIs improve SEO and social shareability
- Next.js auto-generates `<head>` tags
- Only supported in **Server Components**

## Default Fields (always added)

- `<meta charset="utf-8" />`
- `<meta name="viewport" content="width=device-width, initial-scale=1" />`

## Static Metadata

- Export a `metadata` object from `layout.tsx` or `page.tsx`
- Type: `import type { Metadata } from 'next'`
- Fields: `title`, `description`, `openGraph`, `twitter`, etc.

```ts
export const metadata: Metadata = {
  title: 'My Blog',
  description: '...',
}
```

## Dynamic Metadata (`generateMetadata`)

- Async function — can fetch data before returning metadata
- Receives `params` and `searchParams` as props
- Also receives `parent` (ResolvingMetadata) to access parent metadata
- Returns `Promise<Metadata>`

```ts
export async function generateMetadata({ params }: Props): Promise<Metadata> {
  const post = await fetch(`/api/blog/${(await params).slug}`).then(r => r.json())
  return { title: post.title, description: post.description }
}
```

### Streaming Metadata

- Metadata streams separately, doesn't block UI rendering
- **Disabled for bots/crawlers** (Twitterbot, Slackbot, Bingbot) — detected via User Agent
- Customize with `htmlLimitedBots` in `next.config.js`
- Not used for prerendered pages (resolved at build time)

### Memoizing Data Requests

- Use React `cache()` to avoid duplicate fetches between `generateMetadata` and page
- `export const getPost = cache(async (slug) => { ... })`
- Call same function in both `generateMetadata` and component — executes only once

## File-based Metadata

### Favicons

- Place `favicon.ico` in root of `app/` folder
- Can also generate programmatically

### Static OG Images

- Place `opengraph-image.jpg` in `app/` or route folder
- More specific (deeper) image overrides parent
- Supported formats: `.jpg`, `.jpeg`, `.png`, `.gif`
- Also: `twitter-image.jpg` for Twitter cards

### Generated OG Images (`ImageResponse`)

- Create `opengraph-image.tsx` in route folder
- Import: `import { ImageResponse } from 'next/og'`
- Uses JSX + CSS to generate dynamic images
- Export `size` (`width`, `height`) and `contentType`
- Only supports flexbox and subset of CSS (no `display: grid`)
- Powered by `@vercel/og`, `satori`, `resvg`

## Other File Conventions

- `robots.txt` — search engine crawling rules
- `sitemap.xml` — site structure for search engines
- Can be static files or programmatically generated