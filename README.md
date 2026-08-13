# FIXR

**A dual-experience marketplace interface - a warm, consumer-facing customer app and a dark, data-dense provider dashboard, built from one design system.**

[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Vite](https://img.shields.io/badge/Vite-5-646CFF?logo=vite&logoColor=white)](https://vitejs.dev)

---

## Overview

FIXR is a home-services marketplace UI built around a single premise: **customers and providers want opposite things from an interface.**

Customers want warmth, trust signals, and as few decisions as possible. Providers want density, numbers, and speed. Rather than compromise on one neutral design, FIXR ships two visual systems from one component library.

## The dual-token system

Two complete token sets drive the same components:

**Customer - warm editorial, trust-forward**

| Token | Value |
|---|---|
| Background | `#FAFAF7` off-white |
| Ink | `#1A1A2E` |
| Primary | `#0B7B5E` green |
| Accent | `#F59B2B` amber |
| Alert | `#E8453C` red |

**Provider - dark professional, earnings-focused**

| Token | Value |
|---|---|
| Background | `#0F1923` deep navy |
| Surface | `#16222E` |
| Ink | `#F0F4F8` |
| Primary | `#2D8CFF` blue |
| Accent | `#F5A623` gold |

The customer palette leads with green for trust and reserves amber for attention. The provider palette leads with blue for focus and reserves gold for earnings - the number providers actually care about.

## Screenshots

<!-- Add screenshots here:
![Customer experience](docs/screenshots/customer.png)
![Provider dashboard](docs/screenshots/provider.png)
-->

## Features

**Customer experience**

- Category browsing across plumbing, electrical, painting, carpentry, HVAC, roofing, cleaning and windows, with live counts
- Provider discovery with ratings and trust badges
- Job posting flow
- Quote comparison
- Editorial-style layouts with generous whitespace

**Provider experience**

- Dark, data-dense dashboard
- Earnings tracking with gold accent emphasis
- Job feed and bid management
- Compact metric tiles

## Tech stack

| Layer | Technology |
|---|---|
| Framework | React 18 with hooks (`useState`, `useEffect`, `useRef`) |
| Language | TypeScript |
| Styling | Inline design tokens - no CSS framework dependency |
| Build | Vite |

## Getting started

### Prerequisites

- Node.js 18+

### Scaffold and run

```bash
git clone https://github.com/vasanthkumarpulkam/handyman.git
cd handyman

npm create vite@latest . -- --template react-ts
npm install
npm run dev              # http://localhost:5173
```

Then import the FIXR component in `src/main.tsx`:

```tsx
import FixrApp from './FixrApp';
```

## Project structure

```
handyman/
└── src/
    └── FixrApp.tsx       The FIXR interface - design tokens, data, components
```

Vite scaffolding (`main.tsx`, `index.html`, `package.json`, `vite.config.ts`) still needs to be added - see the roadmap.

## Design notes

**Why two palettes instead of a theme toggle?** A theme toggle assumes both audiences want the same information presented differently. They don't. A customer glances at four providers and picks one; a provider scans thirty jobs and needs density. The token split lets the same `Card` or `Badge` component serve both without either side inheriting the other's compromises.

**Why inline tokens rather than Tailwind?** For a design study, having every colour decision visible as a named constant at the top of the file makes the system readable in one pass. In production this would move to CSS custom properties or a Tailwind theme.

## Roadmap

- [x] Move the JSX out of `README.md` into `src/FixrApp.tsx`
- [ ] Add the Vite + React scaffolding so the project builds
- [ ] Split the monolithic component into a proper component tree
- [ ] Extract tokens to CSS custom properties
- [ ] Connect to a real backend

## Related projects

- [Housecal Pro](https://github.com/vasanthkumarpulkam/code-companion-space) - the same marketplace concept, fully implemented on Supabase
- [HandyConnect](https://github.com/vasanthkumarpulkam/studio) - Next.js + Firebase with AI-assisted bidding


## Author

**Vasanth Kumar Pulkam** - [GitHub](https://github.com/vasanthkumarpulkam)
