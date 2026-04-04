# Technology Stack

**Project:** Association Events Platform
**Researched:** 2026-04-04

## Recommended Stack

### Core Framework

| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| Next.js | 16.x | Web application framework | Fits public SEO pages and protected member/staff areas in one codebase. |
| React | 19.x | UI runtime | Standard current pairing with Next.js for interactive member and staff workflows. |
| TypeScript | Current stable | Application language | Strong fit for domain models across events, members, and RSVPs. |

### Data and Auth

| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| PostgreSQL | 16.x family | System of record | Strong relational fit for events, memberships, roles, RSVPs, and reporting. |
| Prisma ORM | 7.x | Data access layer | Good developer ergonomics for relational modeling and migrations. |
| Auth.js | v5 family | Authentication and session management | Supports role-aware auth and database-backed sessions for member and staff access. |

### Supporting Libraries

| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| Zod | 4.x | Shared validation | Validate form input and server-side mutations consistently. |
| React Hook Form | 7.x | Form state | Staff event/member forms and member RSVP-related input flows. |
| @hookform/resolvers | Current | RHF/Zod integration | Keep validation schemas shared instead of duplicating rules. |
| @tanstack/react-table | Current | Staff data tables | RSVP rosters and member management views with filtering and sorting. |
| date-fns | 3.x family | Date formatting and comparisons | Event date rendering, sorting, and display logic. |

## Recommended Architecture Defaults

- Use a modular monolith rather than splitting frontend and backend.
- Keep public browsing, member RSVP, and staff admin in a single app.
- Prefer server-rendered reads for public event pages.
- Use server-side authorization on every member and staff mutation.
- Enforce unique RSVP constraints at the database level.

## Alternatives Considered

| Category | Recommended | Alternative | Why Not |
|----------|-------------|-------------|---------|
| Application shape | Single Next.js app | Separate SPA plus API backend | Adds unnecessary surface area and duplicates auth/validation for v1. |
| Persistence | PostgreSQL | Document database | Weaker fit for joins, constraints, and staff reporting. |
| Auth model | Database-backed sessions | JWT-only auth | Membership status and staff roles may change after login and need server-side checks. |
| Mutation model | Server Actions first | Large REST API surface | Adds overhead for a first-party web app with mostly internal consumers. |

## Why This Fits This Project

This project is a focused workflow product: publish events, gate RSVP by membership, and give staff operational control. The complexity is in access rules, relational data, and operational reliability, not in broad consumer feature breadth. A single Next.js application with PostgreSQL matches that shape directly and keeps the initial release small enough to ship.

## Sources

- https://nextjs.org/docs/app
- https://react.dev/
- https://www.prisma.io/docs
- https://authjs.dev/guides/role-based-access-control
- https://authjs.dev/getting-started/adapters/prisma
- https://zod.dev/v4
- https://react-hook-form.com/
- https://tanstack.com/table/latest/docs/guide/introduction
- https://date-fns.org/
