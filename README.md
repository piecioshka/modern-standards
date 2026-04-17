# Modern Standards

🚀 Production-readiness checklist for modern web applications.

---

If you're missing answers to questions like:

- What should be done before deploying a web application?
- Have I checked everything before showing the application to the world?
- What is important from the perspective of a commercial application?
- What should every application have?

---

## Table of Contents

<!-- To build TOC run: npx markdown-toc -i README.md -->

<!-- toc -->

- [What Every Modern Website Needs](#what-every-modern-website-needs)
- [Frontend (UI)](#frontend-ui)
  * [State Management](#state-management)
  * [Concurrent Requests](#concurrent-requests)
  * [HTTP Request Retry](#http-request-retry)
  * [Response Caching](#response-caching)
  * [Critical Path for Application Loading](#critical-path-for-application-loading)
  * [Component Optimization](#component-optimization)
  * [Loader](#loader)
  * [Test Navigation: "Back" Button in the Browser for SPA Applications](#test-navigation-back-button-in-the-browser-for-spa-applications)
  * [Tools](#tools)
- [Accessibility (a11y)](#accessibility-a11y)
  * [WCAG 2.1 Compliance](#wcag-21-compliance)
  * [Keyboard Navigation](#keyboard-navigation)
  * [Screen Readers](#screen-readers)
  * [Color Contrast](#color-contrast)
  * [Tools](#tools-1)
- [SEO](#seo)
  * [Meta Tags](#meta-tags)
  * [Structured Data](#structured-data)
  * [Sitemap & robots.txt](#sitemap--robotstxt)
  * [Core Web Vitals](#core-web-vitals)
  * [Tools](#tools-2)
- [Internationalization (i18n)](#internationalization-i18n)
  * [Multi-language Support](#multi-language-support)
  * [RTL Layout Support](#rtl-layout-support)
  * [Date, Number & Currency Formatting](#date-number--currency-formatting)
  * [Translation Workflow](#translation-workflow)
- [Backend](#backend)
  * [Local Resources](#local-resources)
  * [Log Management](#log-management)
  * [SSR (Server-Side Rendering)](#ssr-server-side-rendering)
  * [API Design](#api-design)
    + [REST Conventions](#rest-conventions)
    + [API Documentation](#api-documentation)
    + [Rate Limiting](#rate-limiting)
    + [Request Validation](#request-validation)
  * [Database](#database)
    + [Migrations](#migrations)
    + [Connection Pooling](#connection-pooling)
    + [Backups & Restore](#backups--restore)
    + [ORM](#orm)
  * [Tools](#tools-3)
- [Error Handling](#error-handling)
  * [User Input Data](#user-input-data)
  * [Error Page](#error-page)
  * [HTTP Errors: Following the REST Convention](#http-errors-following-the-rest-convention)
  * [HTTP Errors: Defined by the Back-end](#http-errors-defined-by-the-back-end)
  * [HTTP Errors: Malformed Responses](#http-errors-malformed-responses)
- [Security](#security)
  * [OWASP Top 10](#owasp-top-10)
  * [Content Security Policy (CSP)](#content-security-policy-csp)
  * [HTTPS](#https)
  * [Authentication & Authorization](#authentication--authorization)
  * [Secrets Management](#secrets-management)
  * [Dependency Auditing](#dependency-auditing)
- [Tests](#tests)
  * [Offline Connection Tests](#offline-connection-tests)
  * [Performance Tests](#performance-tests)
  * [Tools](#tools-4)
- [Infrastructure](#infrastructure)
  * [Pipelines](#pipelines)
  * [Monitoring & Alerting](#monitoring--alerting)
    + [Error Tracking](#error-tracking)
    + [Application Performance Monitoring (APM)](#application-performance-monitoring-apm)
    + [Uptime Monitoring](#uptime-monitoring)
    + [Alerting Rules](#alerting-rules)
  * [Test Application Rollback Deployment](#test-application-rollback-deployment)
  * [Release Process](#release-process)
- [Codebase (Technical)](#codebase-technical)
  * [Bootstrap Process](#bootstrap-process)
  * [Modules](#modules)
  * [One Place for All Custom Error Types](#one-place-for-all-custom-error-types)
  * [Configuration](#configuration)
  * [Component Events](#component-events)
- [Tools](#tools-5)
  * [Husky](#husky)
  * [Lint-staged](#lint-staged)
  * [Prettier](#prettier)
  * [TypeScript](#typescript)
  * [ESLint](#eslint)
  * [Utilities](#utilities)
  * [HTTP Request](#http-request)
  * [Changelog](#changelog)
- [Developer Experience (DX)](#developer-experience-dx)
  * [EditorConfig](#editorconfig)
  * [Monorepo](#monorepo)
  * [Dev Containers](#dev-containers)
  * [Documentation](#documentation)
  * [Dependency Updates](#dependency-updates)
- [Services](#services)
  * [Sourcegraph](#sourcegraph)
  * [SonarQube](#sonarqube)
  * [GitHub](#github)

<!-- tocstop -->

---

## What Every Modern Website Needs

Beyond the technical stuff, there are a few things every site shown to users in Europe should have. In plain language:

- **Privacy Policy** — what data you collect, why, and how to delete it. One page, linked from the footer.
- **Cookie banner** — if you use analytics, ads, or any third-party tracking. Users must be able to **reject** as easily as accept. Don't load the tracking scripts until they click accept.
- **Contact info** — visible email/address. In Germany & Austria this is required ("Impressum") on every page.
- **Terms of Service** — if users sign up, pay, or upload content.
- **Accessibility** — keyboard navigation works, contrast is readable, screen readers can parse the page. From mid-2025 it's legally required for most B2C sites in the EU.
- **HTTPS everywhere** — no exceptions.
- **A way to delete the account** — if users can register, they must be able to leave.

💡 Tools that handle most of this for you:

- Cookie banners: [orestbida/cookieconsent](https://github.com/orestbida/cookieconsent) (free, open source), [Cookiebot](https://www.cookiebot.com/), [iubenda](https://www.iubenda.com/) (paid, also generate policies).
- Privacy-friendly analytics (no banner needed if configured right): [Plausible](https://plausible.io/), [Umami](https://umami.is/), [Fathom](https://usefathom.com/).
- Privacy policy generators: [iubenda](https://www.iubenda.com/), [TermsFeed](https://www.termsfeed.com/), [Termly](https://termly.io/).
- Payments with VAT handled for you: [Paddle](https://www.paddle.com/), [Lemon Squeezy](https://www.lemonsqueezy.com/), [Stripe Tax](https://stripe.com/tax).
- Accessibility checks: [axe DevTools](https://www.deque.com/axe/devtools/), [Lighthouse](https://developer.chrome.com/docs/lighthouse/), [Pa11y](https://pa11y.org/).

---

## Frontend (UI)

### State Management

- [Zustand](https://zustand.docs.pmnd.rs/) — lightweight, minimal boilerplate, no providers needed
- [Jotai](https://jotai.org/) — atomic state management, great for fine-grained reactivity
- **URL as state** — use search params (`useSearchParams`) for state that should be shareable or bookmarkable (filters, pagination, tabs)
- ~~[Redux](https://redux.js.org/)~~ ❌ — excessive boilerplate (actions, reducers, selectors, middleware) for most applications. Modern alternatives achieve the same result with a fraction of the code.

### Concurrent Requests

- Define the maximum number of simultaneous requests.
- Use request queuing or throttling to avoid overwhelming the server.
- Consider using `Promise.allSettled()` to handle multiple independent requests
  gracefully — it ensures all promises complete regardless of individual failures.

### HTTP Request Retry

Verify:

- Is there a retry mechanism for failed HTTP requests?
- Is the retry limited to idempotent requests (GET, PUT, DELETE) to avoid duplicating side effects?
- Is there a maximum number of retries to prevent infinite loops?
- Is exponential backoff used between retries to avoid server overload?

💡 TIP:

- TanStack Query has built-in retry support with configurable backoff
- For manual implementation, consider:
  - [npm/p-retry](https://www.npmjs.com/package/p-retry)
  - [npm/axios-retry](https://www.npmjs.com/package/axios-retry)

### Response Caching

Verify:

- Is there a caching strategy for HTTP responses?
- Is there a way for the user to clear the cache (e.g., a button in settings)?
- Is cache invalidation properly handled when data changes?
- Is the cache retention time (TTL) defined and appropriate for the data type?

💡 TIP:

- TanStack Query provides built-in caching with configurable `staleTime` and `gcTime`
- Consider using `Cache-Control` HTTP headers for browser-level caching
- For offline support, consider [npm/idb-keyval](https://www.npmjs.com/package/idb-keyval) for IndexedDB-based storage

### Critical Path for Application Loading

Verify:

- Are critical CSS styles inlined in the HTML `<head>` to avoid render-blocking?
- Are non-critical scripts loaded with `defer` or `async` attributes?
- Is code splitting implemented to load only the necessary code for the initial route?
- Are fonts preloaded using `<link rel="preload">`?

💡 TIP:

- Use Lighthouse to audit and measure the critical rendering path
- Lazy-load routes and heavy components with `React.lazy()` and `Suspense`
- Minimize the number of render-blocking resources

### Component Optimization

- Load only what is visible, e.g., images, lists.
  - [Tech] Intersection Observer API
- Cancel HTTP requests if they are no longer needed for the component.
  - [Tech] AbortController

### Loader

Verify:

- Is a loading indicator displayed during data fetching or page transitions?
- Is a skeleton screen used for content areas to improve perceived performance?
- Is there a minimum display time for loaders to prevent flickering?
- Are error and empty states handled when loading completes?

💡 TIP:

- Use skeleton screens instead of spinners for better perceived performance
- Consider [npm/react-content-loader](https://www.npmjs.com/package/react-content-loader) for SVG-based skeleton placeholders

### Test Navigation: "Back" Button in the Browser for SPA Applications

Verify:

- Does the "Back" button navigate to the previous route correctly?
- Is the scroll position restored when navigating back?
- Are form inputs preserved when navigating back (or intentionally cleared)?
- Does deep linking work — can users share or bookmark a URL and land on the correct view?

💡 TIP:

- Test navigation flows manually: navigate forward several pages, then use the "Back" button to verify correct behavior at each step
- Watch for unintended re-fetching or state loss on back navigation

### Tools

- [React](https://react.dev/)
- [Storybook](https://storybook.js.org/)
- [React Hook Form](https://react-hook-form.com/) — performant, flexible form library with minimal re-renders
- ~~[Formik](https://formik.org/)~~ ❌ — largely unmaintained, causes excessive re-renders on every keystroke, and has a larger bundle size compared to React Hook Form

---

## Accessibility (a11y)

### WCAG 2.1 Compliance

Verify:

- Does the application meet at least **WCAG 2.1 Level AA** compliance?
- Are all interactive elements (buttons, links, inputs) accessible?
- Are images and icons accompanied by meaningful `alt` text or `aria-label`?
- Are form fields associated with `<label>` elements?

💡 TIP:

- Reference: <https://www.w3.org/WAI/WCAG21/quickref/>
- Level A = minimum, Level AA = recommended for commercial apps, Level AAA = highest

### Keyboard Navigation

Verify:

- Can every interactive element be reached and operated using only the keyboard?
- Is the focus order logical and follows the visual layout?
- Is there a visible focus indicator (outline) on focused elements?
- Are keyboard traps avoided (user can always Tab away from an element)?
- Is a "Skip to main content" link provided for long navigation menus?

### Screen Readers

Verify:

- Is semantic HTML used (`<nav>`, `<main>`, `<article>`, `<aside>`, `<header>`, `<footer>`) instead of generic `<div>`s?
- Are ARIA roles and attributes used only when native HTML semantics are insufficient?
- Are dynamic content changes announced to screen readers (using `aria-live` regions)?
- Are decorative elements hidden from screen readers (`aria-hidden="true"`)?

### Color Contrast

Verify:

- Does text meet the minimum contrast ratio of **4.5:1** (normal text) or **3:1** (large text)?
- Is color not the only means of conveying information (e.g., error states also use icons or text)?

💡 TIP:

- Test contrast with <https://webaim.org/resources/contrastchecker/>

### Tools

- [axe-core](https://github.com/dequelabs/axe-core) — automated accessibility testing engine
- [eslint-plugin-jsx-a11y](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y) — ESLint rules for accessibility in JSX
- [Lighthouse Accessibility Audit](https://developer.chrome.com/docs/lighthouse/accessibility/) — built into Chrome DevTools
- [NVDA](https://www.nvaccess.org/) (Windows) / VoiceOver (macOS) — screen readers for manual testing

---

## SEO

### Meta Tags

Verify:

- Does every page have a unique `<title>` and `<meta name="description">`?
- Are Open Graph tags set for social media sharing?
  - `og:title`, `og:description`, `og:image`, `og:url`, `og:type`
- Are Twitter Card tags set?
  - `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`
- Is the `<html lang="...">` attribute set to the correct language?
- Is a canonical URL (`<link rel="canonical">`) defined to avoid duplicate content?

### Structured Data

Verify:

- Is JSON-LD structured data added for relevant content types (articles, products, events, FAQ)?
- Does the structured data pass validation?

💡 TIP:

- Reference: <https://schema.org/>
- Validate at <https://validator.schema.org/>
- Test with [Google Rich Results Test](https://search.google.com/test/rich-results)

### Sitemap & robots.txt

Verify:

- Is a `sitemap.xml` generated and submitted to search engines?
- Is `robots.txt` configured to allow indexing of public pages and block private ones?
- Are non-production environments blocked from indexing (`noindex`, `nofollow` or `robots.txt` disallow)?

### Core Web Vitals

Verify:

- **LCP (Largest Contentful Paint)** — is the main content visible within 2.5 seconds?
- **INP (Interaction to Next Paint)** — is the page responsive to user input within 200ms?
- **CLS (Cumulative Layout Shift)** — is the visual stability score below 0.1?

💡 TIP:

- Measure with [PageSpeed Insights](https://pagespeed.web.dev/) or Lighthouse
- Monitor real-user data with [web-vitals](https://www.npmjs.com/package/web-vitals) library

### Tools

- [Lighthouse](https://developer.chrome.com/docs/lighthouse/) — built-in Chrome audit tool
- [Google Search Console](https://search.google.com/search-console/) — monitor indexing and search performance
- [npm/next-seo](https://www.npmjs.com/package/next-seo) — SEO management for Next.js applications

---

## Internationalization (i18n)

### Multi-language Support

Verify:

- Are all user-facing strings externalized into translation files (not hardcoded)?
- Is there a fallback language when a translation key is missing?
- Are translations organized by feature/module for maintainability?
- Is the language preference persisted (URL, cookie, or user settings)?

💡 TIP:

- Use tools:
  - [i18next](https://www.i18next.com/) + [react-i18next](https://react.i18next.com/) — the most popular i18n framework
  - [next-intl](https://next-intl.dev/) — for Next.js applications
  - [FormatJS (react-intl)](https://formatjs.github.io/) — ICU message syntax support

### RTL Layout Support

Verify:

- Does the layout flip correctly for RTL languages (Arabic, Hebrew, Persian)?
- Are CSS logical properties used (`margin-inline-start` instead of `margin-left`)?
- Is the `dir="rtl"` attribute set on the `<html>` element when needed?

### Date, Number & Currency Formatting

Verify:

- Are dates formatted according to the user's locale (e.g., `MM/DD/YYYY` vs `DD.MM.YYYY`)?
- Are numbers formatted with the correct decimal and thousands separators?
- Are currencies displayed with the correct symbol and position?

💡 TIP:

- Use the built-in `Intl` API:
  - `Intl.DateTimeFormat` for dates
  - `Intl.NumberFormat` for numbers and currencies
  - `Intl.RelativeTimeFormat` for relative time ("3 days ago")

### Translation Workflow

Verify:

- Is there a defined process for adding new translations?
- Are missing translations detected automatically (in CI or at runtime)?
- Is there a review process for translations before they go live?

💡 TIP:

- Use translation management platforms: [Crowdin](https://crowdin.com/), [Lokalise](https://lokalise.com/), [Phrase](https://phrase.com/)

---

## Backend

### Local Resources

- Change addresses to local ones to avoid leaving the server room.
- Switch the protocol to HTTP to avoid encrypting local requests.

### Log Management

- Use structured logging with log levels (debug, info, warn, error)
- Include contextual information (request ID, user ID, timestamp) in log entries
- Use tools:
  - [npm/pino](https://www.npmjs.com/package/pino) — fast, low-overhead JSON logger
  - [npm/winston](https://www.npmjs.com/package/winston) — versatile logger with multiple transports
  - ~~[npm/debug](https://www.npmjs.com/package/debug)~~ ❌ — suitable only for development-time debugging, not for production logging. Lacks structured output, log levels, and transport support.

### SSR (Server-Side Rendering)

Verify:

- Does the application benefit from SSR? Consider it when:
  - SEO is important (public-facing content pages)
  - First Contentful Paint (FCP) performance is critical
  - Users on slow devices or networks need faster initial page loads
- Are you handling hydration mismatches between server and client?
- Is sensitive data (tokens, secrets) properly excluded from server-rendered HTML?

💡 TIP:

- Use frameworks with built-in SSR support:
  - [Next.js](https://nextjs.org/) — for React applications
  - [Nuxt](https://nuxt.com/) — for Vue applications
  - [Astro](https://astro.build/) — for content-heavy sites with minimal client-side JS

### API Design

#### REST Conventions

Verify:

- Are resource names plural and lowercase? (e.g., `/api/users`, not `/api/User`)
- Are HTTP methods used correctly? (`GET` for reading, `POST` for creating, `PUT`/`PATCH` for updating, `DELETE` for removing)
- Is API versioning in place? (e.g., `/api/v1/users`)
- Is pagination implemented for list endpoints? (using `page`/`limit` or cursor-based)
- Are consistent response envelopes used? (e.g., `{ data, meta, errors }`)

#### API Documentation

Verify:

- Is there an OpenAPI/Swagger specification for the API?
- Is the documentation auto-generated from code or kept in sync manually?
- Is there a live interactive playground (e.g., Swagger UI)?

💡 TIP:

- Use tools:
  - [Swagger UI](https://swagger.io/tools/swagger-ui/)
  - [Redoc](https://redocly.com/)

#### Rate Limiting

Verify:

- Are API endpoints protected against abuse with rate limiting?
- Are rate limit headers returned to clients? (`X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`)

💡 TIP:

- Use [npm/express-rate-limit](https://www.npmjs.com/package/express-rate-limit) for Express.js

#### Request Validation

Verify:

- Are all incoming request bodies, query params, and path params validated?
- Are validation errors returned with clear, actionable messages?

💡 TIP:

- Use tools:
  - [Zod](https://zod.dev/) — TypeScript-first schema validation with static type inference
  - ~~[Joi](https://joi.dev/)~~ ❌ — no native TypeScript type inference, heavier API surface, and less integration with modern TypeScript-first workflows

### Database

#### Migrations

Verify:

- Is database schema versioned through migration files?
- Can migrations be rolled back safely?
- Are migrations run automatically in the CI/CD pipeline?

#### Connection Pooling

Verify:

- Is a connection pool configured to avoid exhausting database connections?
- Are pool size limits set appropriately for the expected load?

#### Backups & Restore

Verify:

- Are automated backups configured and running?
- Has the restore procedure been tested at least once?
- Is the backup retention policy defined (daily, weekly, monthly)?

#### ORM

- [Prisma](https://www.prisma.io/) — type-safe database client with auto-generated types and migrations
- [Drizzle](https://orm.drizzle.team/) — lightweight, SQL-like TypeScript ORM with zero dependencies

### Tools

- [Node.js](https://nodejs.org/en)
  - [npm/express](https://www.npmjs.com/package/express)
  - [npm/cors](https://www.npmjs.com/package/cors)
  - [npm/helmet](https://www.npmjs.com/package/helmet)
  - [npm/morgan](https://www.npmjs.com/package/morgan)

---

## Error Handling

### User Input Data

Verify:

- Is everything that the user enters into any form field being sanitized?

💡 TIP:

- Use tools:
  - [npm/sanitize-html](https://www.npmjs.com/package/sanitize-html)
  - [npm/escape-html](https://www.npmjs.com/package/escape-html)

### Error Page

Verify:

- Is there a dedicated error page built for the application?
- Are we redirecting the user to an error page when they don't have access
  to the displayed resource? Examples:
  - the page no longer exists,
  - or the user wants to access a page for logged-in users while being logged out

### HTTP Errors: Following the REST Convention

Verify:

- Are we handling problems with obtaining a response due to an HTTP error?
  - e.g., when `HTTP Status 500 - Internal Server Error` occurs

### HTTP Errors: Defined by the Back-end

Verify:

- Are we handling custom errors defined on the server side?
  - e.g., when a resource is not found and we receive a JSON response
    with an `error` key and an error code

💡 TIP:

- Obtain all error codes defined on the server side of the client
  application you are developing

### HTTP Errors: Malformed Responses

Verify:

- Is the response in the correct format?

💡 TIP:

- Use an Output Schema or a Swagger contract where the expected response
  format is defined
- Use tools:
  - ajv — to build a schema for the expected response

---

## Security

### OWASP Top 10

Verify:

- Are you protected against **Cross-Site Scripting (XSS)**? Sanitize all user-generated content before rendering.
- Are you protected against **Cross-Site Request Forgery (CSRF)**? Use anti-CSRF tokens for state-changing operations.
- Are you protected against **SQL Injection**? Use parameterized queries or ORM — never concatenate user input into queries.
- Are you protected against **Broken Authentication**? Enforce strong password policies, implement account lockout, and use MFA where possible.
- Are you protected against **Sensitive Data Exposure**? Encrypt data at rest and in transit, never log sensitive information.

💡 TIP:

- Review the full list: <https://owasp.org/www-project-top-ten/>
- Run automated scans with [OWASP ZAP](https://www.zaproxy.org/)

### Content Security Policy (CSP)

Verify:

- Is a `Content-Security-Policy` header configured to restrict which resources (scripts, styles, images) can be loaded?
- Is `script-src 'unsafe-inline'` avoided? Use nonces or hashes instead.
- Is `frame-ancestors` set to prevent clickjacking?

💡 TIP:

- Start with a report-only policy (`Content-Security-Policy-Report-Only`) to identify violations before enforcing
- Use [helmet](https://www.npmjs.com/package/helmet) in Node.js to set security headers easily
- Test your CSP at <https://csp-evaluator.withgoogle.com/>

### HTTPS

Verify:

- Is HTTPS enforced in production (HTTP redirects to HTTPS)?
- Is HSTS (HTTP Strict Transport Security) enabled with a sufficient `max-age`?
- Are all external resources (APIs, CDNs, fonts) loaded over HTTPS?

### Authentication & Authorization

Verify:

- Are JWT tokens stored securely (prefer `httpOnly` cookies over `localStorage`)?
- Do tokens have a reasonable expiration time?
- Is there a token refresh mechanism?
- Are API endpoints protected with proper authorization checks (not just authentication)?
- Is role-based or attribute-based access control (RBAC/ABAC) implemented consistently?

💡 TIP:

- Consider using established providers: [Auth.js (NextAuth)](https://authjs.dev/), [Clerk](https://clerk.com/), [Auth0](https://auth0.com/)
- Never implement your own cryptography — use well-tested libraries

### Secrets Management

Verify:

- Are secrets (API keys, database credentials, tokens) excluded from the repository?
- Is `.env` listed in `.gitignore`?
- Are production secrets managed through a dedicated service, not environment files?

💡 TIP:

- Use [npm/dotenv](https://www.npmjs.com/package/dotenv) for local development
- Use a secrets manager for production: AWS Secrets Manager, HashiCorp Vault, Doppler
- Run [git-secrets](https://github.com/awslabs/git-secrets) or [gitleaks](https://github.com/gitleaks/gitleaks) in CI to prevent accidental commits

### Dependency Auditing

Verify:

- Is `npm audit` (or equivalent) run regularly?
- Are known vulnerabilities in dependencies addressed promptly?
- Is there a policy for handling critical vs. low-severity vulnerabilities?

💡 TIP:

- Use tools:
  - `npm audit` — built-in, zero setup
  - [Snyk](https://snyk.io/) — continuous monitoring with PR fixes
  - [Socket](https://socket.dev/) — detects supply chain attacks (typosquatting, install scripts)

---

## Tests

### Offline Connection Tests

Steps:

- Disable the internet on the machine where the application is running.

Verify:

- Are we avoiding unnecessary HTTP requests?
- Are we displaying a message about the lack of internet connection?

💡 TIP:

- You can check the internet connection using `navigator.onLine`.

### Performance Tests

Verify:

- How long does it take to handle 100k users?
- How many users can we handle per second?

💡 TIP:

- Use tools like:
  - [k6](https://k6.io/) — modern, developer-friendly load testing tool
  - [Artillery](https://www.artillery.io/)
  - [Apache Benchmark](https://httpd.apache.org/docs/2.4/programs/ab.html)
  - [Locust](https://locust.io/)

### Tools

- Unit Tests
  - [Vitest](https://vitest.dev/) — fast, Vite-native test runner with Jest-compatible API
  - [Jest](https://jestjs.io/docs/testing-frameworks)
- Component Tests
  - [RTL (React Testing Library)](https://testing-library.com/docs/react-testing-library/intro/)
  - ~~[Enzyme](https://enzymejs.github.io/enzyme/)~~ ❌ — no longer maintained, does not support React 18+, and encourages testing implementation details rather than user behavior
- End to End Tests
  - [Playwright](https://playwright.dev/) — recommended, supports all major browsers with a single API
  - [Cucumber.js](https://cucumber.io/docs/guides/overview/)
  - [WebDriver.io](https://webdriver.io/)
  - ~~[Cypress](https://www.cypress.io/)~~ ❌ — limited to Chromium-based browsers for full support, lacks native multi-tab testing, and has a restrictive free-tier dashboard
- Code Coverage
  - [npm/c8](https://www.npmjs.com/package/c8) — uses V8's built-in code coverage, fast and accurate
  - [npm/nyc](https://www.npmjs.com/package/nyc)
  - ~~[istanbul](https://istanbul.js.org/)~~ ❌ — succeeded by nyc and c8, which provide better integration and performance

---

## Infrastructure

### Pipelines

Every CI/CD pipeline should include these steps:

1. **Install** — install dependencies (`npm ci` for deterministic installs)
2. **Lint** — run ESLint and Prettier checks
3. **Type Check** — run `tsc --noEmit`
4. **Test** — run unit, component, and integration tests
5. **Build** — create production build
6. **E2E Tests** — run end-to-end tests against the built application
7. **Deploy** — deploy to the target environment

Additional checks:

- **UI Error Collector** of runtime errors
- **Observability**
  - Logs
  - Metrics
  - Traces

💡 TIP:

- Use [GitHub Actions](https://github.com/features/actions) for CI/CD
- Use [Docker](https://www.docker.com/) for reproducible builds and deployments
- Cache `node_modules` and build artifacts between pipeline runs to speed up CI

### Monitoring & Alerting

#### Error Tracking

Verify:

- Are runtime errors automatically captured and reported?
- Is source map upload configured so stack traces show original code?
- Are errors grouped and deduplicated to avoid noise?

💡 TIP:

- Use tools:
  - [Sentry](https://sentry.io/) — real-time error tracking with release tracking and performance monitoring
  - [Bugsnag](https://www.bugsnag.com/) — error monitoring with stability scores

#### Application Performance Monitoring (APM)

Verify:

- Are response times and throughput tracked for API endpoints?
- Are slow queries and bottlenecks identified?
- Are custom metrics defined for business-critical operations?

💡 TIP:

- Use tools:
  - [Datadog](https://www.datadoghq.com/)
  - [New Relic](https://newrelic.com/)
  - [Grafana](https://grafana.com/) + [Prometheus](https://prometheus.io/) — open-source alternative

#### Uptime Monitoring

Verify:

- Is the application's availability monitored from external locations?
- Are stakeholders alerted when downtime occurs?

💡 TIP:

- Use tools:
  - [Better Uptime](https://betteruptime.com/)
  - [Pingdom](https://www.pingdom.com/)
  - [UptimeRobot](https://uptimerobot.com/) — free tier available

#### Alerting Rules

Verify:

- Are alerting thresholds defined (error rate, response time, CPU/memory usage)?
- Is there a clear escalation path (who gets paged first, how to escalate)?
- Are alerts actionable — does each alert have a runbook or link to documentation?

### Test Application Rollback Deployment

Verify:

- Can the application be rolled back to a previous version quickly?
- Is there an automated rollback mechanism in case of a failed deployment?
- Have you tested the rollback procedure to ensure it works correctly?

💡 TIP:

- Practice rollback deployments regularly — not just when something goes wrong
- Ensure database migrations are backward-compatible so rollback doesn't break the data layer
- Use blue-green or canary deployment strategies to minimize rollback risk

### Release Process

- Follow [Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH)
- Use [Conventional Commits](https://www.conventionalcommits.org/) for consistent commit messages (`feat:`, `fix:`, `chore:`, `docs:`, `refactor:`)
- Auto-generate changelogs from commit history
- Use tools for application deployment:
  - [npm/release-it](https://www.npmjs.com/package/release-it)
  - [npm/changesets](https://www.npmjs.com/package/@changesets/cli) — for monorepo versioning
  - [npm/semantic-release](https://www.npmjs.com/package/semantic-release) — fully automated version management and publishing

---

## Codebase (Technical)

### Bootstrap Process

- Having one function that starts the application e.g. `main()`
- Implement **graceful shutdown** — handle `SIGTERM`/`SIGINT` signals to close database connections, finish pending requests, and clean up resources before the process exits
- Add a **health check** endpoint (`/health` or `/healthz`) that returns the application's status — used by load balancers, orchestrators (Kubernetes), and monitoring tools

### Modules

- ES Modules (ESM) — the standard module system for modern JavaScript
- Bundlers:
  - [Vite](https://vite.dev/) — fast dev server with HMR and optimized production builds
  - [esbuild](https://esbuild.github.io/) — extremely fast JavaScript bundler
  - ~~[webpack](https://webpack.js.org/)~~ ❌ — significantly slower build times compared to modern alternatives, complex configuration, and being gradually replaced in the ecosystem by Vite and Turbopack

### One Place for All Custom Error Types

- Define all errors in a single location to avoid scattering them across the application.
- Example: `src/errors/index.ts`

### Configuration

- <https://12factor.net/pl/config>
  - Having one place with configuration `config.js`
- Validate environment variables at startup — fail fast if required variables are missing

💡 TIP:

- Use [t3-env](https://env.t3.gg/) for type-safe environment variable validation with Zod
- Use [Zod](https://zod.dev/) schemas to validate and parse configuration at build/start time

### Component Events

- Communication between components [npm/super-event-emitter](https://www.npmjs.com/package/super-event-emitter)

---

## Tools

### Husky

- <https://typicode.github.io/husky/>

### Lint-staged

- <https://github.com/okonet/lint-staged>

### Prettier

- <https://prettier.io/>

### TypeScript

- Enable "Strict Mode" in `tsconfig.json`

  ```json
  {
    "compilerOptions": {
      "strict": true
    }
  }
  ```

### ESLint

🕹️ [Playground](https://eslint.org/play/)

```js
'no-unsafe-optional-chaining': 'error',
'prefer-arrow-callback': 'error',
'no-param-reassign': 'error',
'no-extra-boolean-cast': 'error',
```

plugins:

- [eslint-plugin-import-helpers](https://github.com/willhoney7/eslint-plugin-import-helpers)
- [eslint-plugin-import](https://github.com/import-js/eslint-plugin-import)
  - [consistent-type-specifier-style](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/consistent-type-specifier-style.md)
    ```js
    'import/consistent-type-specifier-style': [
      'error',
      {
        mode: 'prefer-top-level',
      },
    ],
    ```
  - [enforce-node-protocol-usage](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/enforce-node-protocol-usage.md)
    ```js
    'import/enforce-node-protocol-usage': 'error',
    ```
  - [newline-after-import](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/newline-after-import.md)
    ```js
    'import/newline-after-import': ["error", { "count": 1 }],
    ```
  - [no-extraneous-dependencies](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-extraneous-dependencies.md)
    ```js
    'import/no-extraneous-dependencies': [
      'error',
      {
        devDependencies: false,
        optionalDependencies: false,
        peerDependencies: false,
      },
    ],
    ```
  - [no-relative-parent-imports](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-relative-parent-imports.md)
    ```js
    'import/no-relative-parent-imports': 'error',
    ```
  - [no-self-import](https://github.com/import-js/eslint-plugin-import)
    ```js
    'import/no-self-import': 'error',
    ```
  - [order](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/order.md)
    ```js
    'import/order': [
      'error',
      {
        groups: [
          'builtin',
          'external',
          'internal',
          'unknown',
          'parent',
          'sibling',
          'index',
          'object',
          'type',
        ],
        'newlines-between': 'always',
      },
    ],
    ```
- [eslint-plugin-no-relative-import-paths](https://github.com/MelvinVermeer/eslint-plugin-no-relative-import-paths)
- [eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react)
- [eslint-plugin-regexp](https://github.com/ota-meshi/eslint-plugin-regexp)
- [eslint-plugin-smells](https://github.com/elijahmanor/eslint-plugin-smells)
- [eslint-plugin-unicorn](https://github.com/sindresorhus/eslint-plugin-unicorn)
- [eslint-plugin-unused-imports](https://github.com/sweepline/eslint-plugin-unused-imports)

TypeScript plugins:

- [@typescript-eslint/array-type](https://typescript-eslint.io/rules/array-type/)
- [@typescript-eslint/explicit-member-accessibility](https://typescript-eslint.io/rules/explicit-member-accessibility/)
- [@typescript-eslint/no-floating-promises](https://typescript-eslint.io/rules/no-floating-promises/)
- [@typescript-eslint/no-explicit-any](https://typescript-eslint.io/rules/no-explicit-any/)
- [@typescript-eslint/consistent-type-imports](https://typescript-eslint.io/rules/consistent-type-imports/)
  - <https://typescript-eslint.io/blog/consistent-type-imports-and-exports-why-and-how/#benefits-of-enforcing-type-only-importsexports>

### Utilities

- [Lodash](https://lodash.com/) - the best is version "lodash-es" because it supports Tree Shaking

### HTTP Request

- [TanStack Query (React Query)](https://tanstack.com/query/latest) - declarative data fetching with caching, refetching, and synchronization
- [Apollo GraphQL](https://www.apollographql.com/) - for GraphQL APIs
- ~~[Axios](https://axios-http.com/)~~ ❌ — the native `fetch` API is now well-supported across all modern browsers and Node.js 18+, making Axios largely unnecessary. `fetch` is lighter, has no dependencies, and supports streaming natively.

### Changelog

- [changelog-all-possibilities](https://github.com/piecioshka/changelog-all-possibilities)
- [🇵🇱 Blogpost: husky-commitlint-git-changelog](https://piecioshka.pl/blog/2019/03/23/husky-commitlint-git-changelog.html)

---

## Developer Experience (DX)

### EditorConfig

- Add an `.editorconfig` file to ensure consistent formatting across different editors and IDEs

```ini
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = false
```

- Reference: <https://editorconfig.org/>

### Monorepo

Verify:

- Does the project benefit from a monorepo structure (shared code, multiple packages/apps)?
- Is a monorepo tool configured to manage dependencies and task orchestration?

💡 TIP:

- Use tools:
  - [Turborepo](https://turbo.build/) — fast, incremental builds with remote caching
  - [Nx](https://nx.dev/) — smart builds, code generation, and dependency graph visualization

### Dev Containers

- Define a `.devcontainer/` configuration for consistent development environments
- Ensures all team members have the same tools, extensions, and settings

💡 TIP:

- Reference: <https://containers.dev/>
- Supported by VS Code, GitHub Codespaces, and JetBrains IDEs

### Documentation

- **ADRs (Architecture Decision Records)** — document significant technical decisions with context, options considered, and rationale
  - Store in `docs/adr/` directory
  - Reference: <https://adr.github.io/>
- **Onboarding guide** — a README or wiki page that helps new developers set up the project and understand the architecture
- **API documentation** — keep API docs close to the code (OpenAPI/Swagger)

### Dependency Updates

Verify:

- Is there an automated process for updating dependencies?
- Are major version updates reviewed manually before merging?
- Are dependency update PRs tested by CI before merging?

💡 TIP:

- Use tools:
  - [Renovate](https://www.mend.io/renovate/) — highly configurable, supports grouping and auto-merge for patch updates
  - [Dependabot](https://docs.github.com/en/code-security/dependabot) — built into GitHub, simple setup

---

## Services

### Sourcegraph

- <https://sourcegraph.com/search>

### SonarQube

- <https://www.sonarsource.com/products/sonarqube/>

### GitHub

- Template for PR - `.github/PULL_REQUEST_TEMPLATE.md`
  - <https://github.com/devspace/awesome-github-templates#rocket-templates-for-pull-requests>
- Template for issues - `.github/ISSUE_TEMPLATE.md`
  - <https://github.com/devspace/awesome-github-templates#bomb-templates-for-issues>
- Contributing rules - `.github/CONTRIBUTING.md`
