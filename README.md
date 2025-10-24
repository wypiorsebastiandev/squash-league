## SquashLeague

![Node](https://img.shields.io/badge/node-22.14.0-339933?logo=node.js&logoColor=white)
![Astro](https://img.shields.io/badge/Astro-5-FF5D01?logo=astro&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=0b1)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-4-38B2AC?logo=tailwindcss&logoColor=white)
![License](https://img.shields.io/badge/license-TBD-lightgrey)

### Project description

SquashLeague is a Progressive Web App (PWA) that digitizes and streamlines how match results are recorded in an amateur squash league. Players can submit and edit match scores online, while admins manage seasons, rounds, groups, and scoring in a single place. The app is designed to be fast (Astro + React), mobile‑first, and available offline for key flows.

Key highlights:

- Player login and result submission with per‑set scores and validation (5 sets, to 11, win by 2)
- Instant table recalculation and points allocation per match outcome (including walkovers)
- Admin panel for seasons, rounds, groups, players, manual WO, audit trail, and re‑calculation
- Notifications via email, web push, and SMS with rate limiting
- Offline support for “My matches” and the result form with conflict handling

---

### Table of contents

- [Project description](#project-description)
- [Tech stack](#tech-stack)
- [Getting started locally](#getting-started-locally)
- [Available scripts](#available-scripts)
- [Project scope](#project-scope)
- [Project status](#project-status)
- [License](#license)

---

### Tech stack

- Astro 5 (primary web framework)
- React 19 for interactive components
- TypeScript 5 for static typing
- Tailwind CSS 4 for styling
- shadcn/ui for accessible React UI components
- Supabase (PostgreSQL, Auth) as the backend platform
- GitHub Actions for CI/CD (planned)
- DigitalOcean for hosting via Docker image (planned)
- OpenRouter.ai for flexible AI model access (planned optional)

See also: `.ai/tech-stack.md` and `.ai/prd.md` for context.

---

### Getting started locally

Prerequisites:

- Node.js 22.14.0 (see `.nvmrc`)
- npm (comes with Node)

Clone and run:

```bash
git clone <this-repo-url>
cd squash-league

# Use the right Node version
nvm use 22.14.0  # or install Node 22.14.0 via your preferred method

# Install dependencies
npm install

# Start dev server
npm run dev
```

Build and preview production build:

```bash
npm run build
npm run preview
```

Code quality:

```bash
# Lint (ESLint)
npm run lint

# Lint and fix
npm run lint:fix

# Format (Prettier)
npm run format
```

Environment variables:

- None are required for the current starter to run locally.
- Supabase integration will require typical variables (e.g., `SUPABASE_URL`, `SUPABASE_ANON_KEY`). These will be documented alongside the integration.

---

### Available scripts

- `npm run dev`: Start the Astro dev server
- `npm run build`: Build the production site
- `npm run preview`: Preview the production build locally
- `npm run astro`: Run Astro CLI directly
- `npm run lint`: Run ESLint across the project
- `npm run lint:fix`: Run ESLint with automatic fixes
- `npm run format`: Format files using Prettier (with `prettier-plugin-astro`)

---

### Project scope

In scope for MVP:

- User registration and login by invitation (Supabase Auth)
- Seasons, rounds, and groups management
- Automatic round‑robin pairing within groups
- Result submission and editing (time‑boxed)
- Automatic scoring and table updates
- Notifications (email/SMS/web push)
- Offline PWA support (key views and forms)
- Admin dashboard and audit trail

Out of scope for MVP:

- Automatic promotions/relegations between rounds
- External calendar or payment integrations
- Comments or social features
- Public (non‑authenticated) standings view
- CSV export
- Court booking integrations

For detailed functional requirements and user stories, see `.ai/prd.md`.

---

### Project status

- Status: MVP scaffold in progress (Astro + React + TS + Tailwind baseline in place)
- CI/CD: Planned with GitHub Actions
- Hosting: Planned on DigitalOcean via Docker image
- PWA: Offline support planned per PRD
- Admin dashboard: Planned per PRD

Useful docs:

- Product Requirements: `.ai/prd.md`
- Tech stack overview: `.ai/tech-stack.md`

---

### License

TBD. No license has been specified yet. Until a license is added, all rights are reserved. If you plan to open‑source, consider adding a license (see `https://choosealicense.com`).
