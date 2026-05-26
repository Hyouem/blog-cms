<div align="center">

![Angular](https://img.shields.io/badge/Angular-17.2-DD0031?style=for-the-badge&logo=angular&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.3-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![SSR](https://img.shields.io/badge/Angular%20SSR-✓-DD0031?style=for-the-badge&logo=angular&logoColor=white)
![SCSS](https://img.shields.io/badge/Styles-SCSS-CC6699?style=for-the-badge&logo=sass&logoColor=white)
![OneEntry](https://img.shields.io/badge/OneEntry-Headless%20CMS-6C63FF?style=for-the-badge&logoColor=white)

**A blog frontend built with Angular 17 + Server-Side Rendering, powered by OneEntry as a Headless CMS.**  
Content managed in the cloud, delivered fast via SSR — no backend code required.

</div>

---
# Blog CMS

## Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [OneEntry Setup](#oneentry-setup)
  - [Environment Variables](#environment-variables)
  - [Installation & Running](#installation--running)
- [Available Scripts](#available-scripts)
- [How SSR Works](#how-ssr-works)
- [Contributing](#contributing)

---

## Overview

**Blog CMS** is an Angular 17 application that demonstrates how to build a content-driven blog using a **Headless CMS** instead of a traditional backend. All content — posts, pages, and media — is managed through [OneEntry](https://oneentry.cloud)'s admin panel and fetched at render time via the `oneentry` npm SDK.

Because it uses **Angular SSR** (`@angular/ssr` + Express), pages are server-rendered before reaching the browser, giving you faster initial loads and better SEO out of the box.

---

## Tech Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | Angular | ^17.2 |
| Language | TypeScript | ~5.3 |
| Styles | SCSS | — |
| SSR Runtime | Angular SSR + Express | ^17.2.3 / ^4.18.2 |
| CMS | OneEntry Headless CMS | via `oneentry` SDK |
| Reactivity | RxJS | ~7.8 |
| Tests | Karma + Jasmine | ~6.4 / ~5.1 |

---

## Project Structure

```
blog-cms/
├── src/
│   ├── app/
│   │   ├── components/        # Reusable UI components (cards, header, etc.)
│   │   ├── pages/             # Route-level views (home, post detail, etc.)
│   │   ├── services/          # OneEntry SDK integration & data fetching
│   │   ├── app.component.ts
│   │   ├── app.config.ts
│   │   ├── app.config.server.ts
│   │   └── app.routes.ts
│   ├── assets/
│   └── styles.scss
├── server.ts                  # Express SSR server
├── angular.json
├── package.json
└── tsconfig.json
```

---

## Getting Started

### Prerequisites

- Node.js 18+
- npm 9+
- Angular CLI 17 — `npm install -g @angular/cli`
- A [OneEntry](https://oneentry.cloud) account

### OneEntry Setup

1. Sign up at [oneentry.cloud](https://oneentry.cloud) and create a new project.
2. In your project settings, go to **API Tokens** and generate a new token.
3. Structure your content in the OneEntry admin panel — create a **Page** or **Block** type for your blog posts with the fields you need (title, body, cover image, etc.).

> OneEntry's [official docs](https://doc.oneentry.cloud) walk through content modeling in detail.

### Environment Variables

Create a `.env` file (or set these in your shell) before running the app:

```env
ONEENTRY_URL=https://your-project.oneentry.cloud
ONEENTRY_TOKEN=your_api_token_here
```

> **Never commit your token.** Add `.env` to `.gitignore`.

### Installation & Running

```bash
# 1. Clone the repository
git clone https://github.com/Hyouem/blog-cms.git
cd blog-cms

# 2. Install dependencies
npm install

# 3. Start the development server
npm start
```

The app will be available at `http://localhost:4200`.

---

## Available Scripts

| Command | Description |
|---------|-------------|
| `npm start` | Start dev server at `http://localhost:4200` |
| `npm run build` | Build for production (outputs to `dist/`) |
| `npm run watch` | Build in watch mode for development |
| `npm test` | Run unit tests via Karma |
| `npm run serve:ssr:blog-cms` | Serve the SSR production build via Node/Express |

---

## How SSR Works

This project uses **Angular SSR** (`@angular/ssr`) with a Node.js Express server (`server.ts`). The flow at runtime:

```
Browser request
      │
      ▼
Express server (server.ts)         ← listens on PORT env var, default 4000
      │
      ▼
Angular CommonEngine              ← renders the requested route server-side
      │  fetches content from OneEntry CMS during render
      ▼
Fully rendered HTML               ← sent to the browser
      │
      ▼
Angular hydrates the page         ← takes over as a normal SPA
```

Static assets are served directly by Express with a 1-year cache header. All other routes go through the Angular engine.

**To run the SSR production server:**

```bash
# Build first
npm run build

# Then serve
npm run serve:ssr:blog-cms
```

The server will listen at `http://localhost:4000` (or the value of the `PORT` environment variable).

---

## Contributing

Contributions are welcome! To get started:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'feat: add your feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request
