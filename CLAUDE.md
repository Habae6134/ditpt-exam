# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-page PWA for a practical exam lottery system (실기시험 제비뽑기). No build system — the entire app is one HTML file (`index.html`) with inline CSS and JS.

## Dev Server

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

## Deployment

Deployed via Vercel, connected to the `main` branch of GitHub (`Habae6134/ditpt-exam`). Pushing to `main` triggers automatic redeployment.

## Architecture

**Single file app**: All logic, styles, and markup live in `index.html`. There is no framework, bundler, or build step.

**Two screens**: `#screenCategory` (subject picker) → `#screenDraw` (lottery + item management). Visibility is toggled via `.active` CSS class.

**Two modes**:
- `number` mode (신경해부생리학): draws random numbers from a range
- `text` mode (운동치료학, 기능해부학): draws from a list of named items stored in Supabase

**Supabase tables**:
- `questions` — stores exam items per category (`id`, `category`, `name`)
- `saved_lists` — stores named save slots per category (`slot`, `category`, `name`, `items[]`)

The Supabase client is initialized with a publishable key directly in `index.html`. All DB reads/writes happen client-side.

## Service Worker & Caching

`sw.js` caches the app shell. The cache version (`ditpt-v1`, `ditpt-v2`, etc.) must be bumped whenever a new deployment needs to invalidate the browser cache. Without bumping the version, users will continue to see the old cached version.

## Working Principles

- **Plan first, execute after approval**: Always show a plan before making any code changes, DB modifications, or git operations. Only proceed when the user explicitly approves.
