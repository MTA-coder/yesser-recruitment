# YSSR — Yesser Project Angular App

> A modular, enterprise-grade Angular 15 web application featuring bilingual (Arabic / English) support, role-based access control, Firebase push notifications, and a rich suite of business modules.

---

## Table of Contents

- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Available Scripts](#available-scripts)
- [Project Architecture](#project-architecture)
- [Feature Modules](#feature-modules)
- [Core Module](#core-module)
- [Internationalization (i18n)](#internationalization-i18n)
- [Environments](#environments)
- [Build & Deployment](#build--deployment)
- [Bundle Analysis](#bundle-analysis)
- [Contributing](#contributing)

---

## Tech Stack

| Layer | Technology |
| --- | --- |
| Framework | Angular 15 |
| Language | TypeScript 4.9 (strict mode) |
| UI Components | Angular Material 15 |
| Data Grids | AG Grid 29 |
| Rich Text Editor | CKEditor 5 |
| State / Lifecycle | @ngneat/until-destroy |
| Internationalization | @ngx-translate/core |
| Date Handling | Luxon · hijri-converter |
| Charts | Chart.js 4 |
| Push Notifications | Firebase Cloud Messaging |
| Input Masking | ngx-mask 15 |
| Styling | SCSS with custom Material theme |
| Testing | Jasmine · Karma |
| Build | Angular CLI · Webpack |

---

## Prerequisites

- **Node.js** >= 16.x
- **npm** >= 8.x (or **yarn** >= 1.22)
- **Angular CLI** 15.x — install globally:

```bash
npm install -g @angular/cli@15
```

---

## Getting Started

```bash
# 1. Clone the repository
git clone https://gitlab.com/besher.rehawee/yeserprojectAngularApp.git
cd yeserprojectAngularApp

# 2. Install dependencies
npm install

# 3. Start the development server
npm start
# → App runs at http://localhost:4200
```

---

## Available Scripts

| Command | Description |
| --- | --- |
| `npm start` | Serve the app with live-reload (`ng serve`) |
| `npm run bismillah` | Serve without live-reload (useful for stable local testing) |
| `npm run build` | Production build → `dist/yssr` |
| `npm run watch` | Build in watch mode (development config) |
| `npm test` | Run unit tests via Karma |
| `npm run build:stats` | Production build with Webpack stats JSON |
| `npm run analyze` | Open Webpack Bundle Analyzer on the latest build |

---

## Project Architecture

The application follows Angular best practices with a clear separation of concerns:

```
src/
├── app/
│   ├── app.module.ts              # Root module — bootstraps the application
│   ├── app-routing.module.ts      # Top-level routes with lazy loading
│   ├── core/                      # Singleton services, guards, interceptors, shared components
│   │   ├── components/            # Reusable UI components (grid, loader, toast, etc.)
│   │   ├── constants/             # Application-wide constants & enums
│   │   ├── directives/            # Structural & attribute directives
│   │   ├── guards/                # Auth & role-based route guards
│   │   ├── interceptors/          # HTTP interceptor (auth tokens, error handling)
│   │   ├── interfaces/            # TypeScript interfaces & type definitions
│   │   ├── models/                # Domain models
│   │   ├── pipes/                 # Custom pipes (date, highlight, URL, etc.)
│   │   ├── services/              # Singleton services (API, permission, etc.)
│   │   └── validators/            # Custom form validators
│   └── modules/                   # Lazy-loaded feature modules
├── assets/
│   ├── i18n/                      # Translation files (ar.json, en.json)
│   ├── images/
│   ├── styles/                    # Global SCSS partials & theme
│   └── svg/
└── environments/                  # Environment-specific configuration
```

### Key Architectural Decisions

- **Lazy Loading** — Every feature module is lazy-loaded via `loadChildren` to keep the initial bundle small.
- **Core Module** — Imported once in `AppModule`; contains all singleton providers, shared components, directives, and pipes.
- **Strict TypeScript** — `strict: true`, `noImplicitReturns`, `strictTemplates`, and all additional strictness flags are enabled.
- **HTTP Interceptor** — Centralized request/response handling for authentication tokens, error interception, and loading state.
- **Route Guards** — `AuthGuard` and `RoleGuard` protect routes based on authentication state and user permissions.

---

## Feature Modules

All business modules live under `src/app/modules/` and are lazy-loaded:

| Module | Description |
| --- | --- |
| `auth` | Login, registration, and authentication flows |
| `branches` | Branch management |
| `complaints` | Complaint tracking and resolution |
| `customers` | Customer management |
| `delegations` | Delegation workflows |
| `employees` | Employee records and management |
| `file` | File upload and document management |
| `financial-management` | Financial operations and reporting |
| `home` | Dashboard and landing page |
| `housing` | Housing management |
| `managing-operations` | Operations management |
| `offices` | Office management |
| `recruitment-contracts` | Recruitment and contract management |
| `refund-management` | Refund processing |
| `rent` | Rental management |
| `settings` | Application settings and configuration |
| `surety-transfer` | Surety transfer workflows |
| `users` | User management and roles |
| `waiver` / `waiver-specifications` | Waiver processing |
| `worker` / `worker-benefits` | Worker records and benefit management |

---

## Core Module

The `CoreModule` provides a rich set of reusable building blocks:

### Shared Components
- **AG Custom Grid** — Wrapper around AG Grid with custom cell renderers (text, radio, select, date range, number range), header renderers, pagination, and filter templates.
- **Mat Custom Field** — Enhanced Material form fields.
- **Searchable Select** — Autocomplete dropdown with search filtering.
- **Permission Table** — Dynamic permission matrix with checkbox cells.
- **Loader / Loader Data** — Global and inline loading indicators.
- **Toast / Notifier** — Notification system for success, error, and info messages.
- **Nav Breadcrumb** — Dynamic breadcrumb navigation.
- **Lookup Search** — Reusable lookup/search dialogs.
- **Validation Fields** — Centralized form validation display.

### Custom Directives
- `AuthorizationDirective` — Show/hide elements based on user permissions.
- `FlipDirectionDirective` — Handle RTL/LTR layout flipping.
- `InfiniteScrollDirective` — Infinite scroll for lists and tables.

### Custom Pipes
- `TimePipe`, `FullUrlPipe`, `HighlightPipe`, `SanitizerUrlPipe`, `TimezoneToDatePipe`, `HijriGregorianDatePipe`

---

## Internationalization (i18n)

The app uses **@ngx-translate** for runtime language switching between **Arabic** and **English**.

- Translation files: `src/assets/i18n/ar.json` and `src/assets/i18n/en.json`
- RTL/LTR layout is handled via the `FlipDirectionDirective` and SCSS mixins.
- Hijri (Islamic) calendar support is provided via the `hijri-converter` package and custom `HijriGregorianDatePipe`.

---

## Environments

| File | Purpose |
| --- | --- |
| `environment.ts` | Default / local development |
| `environment.dev.ts` | Development / staging server |
| `environment.prod.ts` | Production |

Each environment exposes: `production` flag, `baseURL`, `serverURL`, and Firebase configuration.

---

## Build & Deployment

### Development

```bash
npm start
```

### Production Build

```bash
npm run build
```

Output is emitted to `dist/yssr/` with hashed filenames for cache-busting. A `postbuild` script runs automatically after the build.

### Serve Production Build Locally

```bash
npx http-server dist/yssr -o
```

---

## Bundle Analysis

To inspect the production bundle size and composition:

```bash
npm run build:stats
npm run analyze
```

This opens the Webpack Bundle Analyzer in your browser, allowing you to identify large dependencies and optimization opportunities.

---

## Contributing

1. Create a feature branch from `main`:
   ```bash
   git checkout -b feature/your-feature-name
   ```
2. Follow the existing code style and module patterns.
3. Ensure strict TypeScript compilation passes with no errors.
4. Write or update unit tests for any new functionality.
5. Submit a Merge Request targeting `main` with a clear description of changes.

### Code Style Guidelines

- Use **lazy-loaded modules** for all new feature areas.
- Place shared/reusable code in `core/`.
- Use `@ngneat/until-destroy` for subscription management — avoid manual `unsubscribe()`.
- All new components must use `OnPush` change detection where possible.
- Use translation keys for all user-facing strings — never hardcode text.
