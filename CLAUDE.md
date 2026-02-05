# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run dev` - Start development server (http://localhost:3000)
- `npm run build` - Build for production
- `npm run start` - Start production server
- `npm run lint` - Run ESLint

## Architecture

This is a Next.js 16 project using the App Router with:
- **TypeScript** with strict mode enabled
- **Tailwind CSS 4** for styling
- **React 19**

### Project Structure

- `src/app/` - App Router pages and layouts
- `src/app/layout.tsx` - Root layout with Geist font configuration
- `src/app/page.tsx` - Home page
- `src/app/globals.css` - Global styles and Tailwind imports

### Path Aliases

Use `@/*` to import from `src/*` (e.g., `import { Component } from "@/components/Component"`)
