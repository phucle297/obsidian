# Next.js Linking and Navigating

## Overview
- Routes are server-rendered by default → client waits for server response
- Next.js uses prefetching, streaming, and client-side transitions to keep navigation fast

## `<Link>` Component
- Import: `import Link from 'next/link'`
- Replaces `<a>` tag for internal navigation
- Enables client-side transitions (no full page reload)
- Auto-prefetches routes when link enters viewport

## How Navigation Works

### Server Rendering
- Layouts and Pages are React Server Components by default
- Two types:
  - **Prerendering** — at build time or during revalidation, result is cached
  - **Dynamic Rendering** — at request time

### Prefetching
- Loads route in background before user navigates
- Auto-triggered when `<Link>` enters viewport
- **Static route** → full route prefetched
- **Dynamic route** → skipped or partially prefetched (if `loading.tsx` exists)
- Disable: `<Link prefetch={false} href="...">`
- Hover-only prefetch pattern available for large link lists

### Streaming
- Server sends parts of dynamic route as they're ready
- Enabled by adding `loading.tsx` in route folder
- Next.js auto-wraps `page.tsx` in `<Suspense>` boundary
- Benefits: immediate navigation, interactive layouts, better TTFB/FCP/TTI

### Client-side Transitions
- `<Link>` updates content dynamically instead of full page reload
- Keeps shared layouts and UI intact
- Replaces current page with prefetched loading state or new page
- Makes server-rendered apps feel like SPAs

## Common Slow Transition Causes

### Dynamic routes without `loading.tsx`
- Client waits for full server response → feels unresponsive
- Fix: add `loading.tsx` to enable partial prefetching

### Dynamic segments without `generateStaticParams`
- Route falls back to dynamic rendering at request time
- Fix: add `generateStaticParams` to prerender at build time

### Slow networks
- Prefetch may not finish before user clicks
- Fix: use `useLinkStatus` hook for immediate visual feedback

### Hydration not completed
- Large JS bundles delay hydration → delays prefetching
- Fix: reduce bundle size, move logic to server

## `useLinkStatus` Hook
- `import { useLinkStatus } from 'next/link'`
- Returns `{ pending }` — true while transition is in progress
- Useful for showing loading indicators on slow networks

## Native History API
- `window.history.pushState` — add new entry (user can go back)
- `window.history.replaceState` — replace current entry (no back)
- Integrates with Next.js Router (`usePathname`, `useSearchParams`)
- Use case: sorting, filtering, locale switching without full navigation