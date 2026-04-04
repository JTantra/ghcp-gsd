# OrgHub — Professional Association Management Platform

## What This Is

A web platform for professional associations to manage their membership, events, and member resources. Members can sign up, browse and RSVP to events, access members-only content, and connect via a member directory. Admins manage events, content, and the membership lifecycle from a dedicated interface.

## Core Value

Members can discover, RSVP to, and attend events run by their professional association — this is the heartbeat of why anyone joins.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Members can register and create a profile
- [ ] Members can browse upcoming events (conferences, workshops, meetups)
- [ ] Members can RSVP to events
- [ ] Members can access members-only content (resources, recordings)
- [ ] Members can view a member directory and network
- [ ] Members-only events are gated behind membership
- [ ] Admins can create and manage events (multiple types)
- [ ] Admins can manage members (view, approve, deactivate)
- [ ] Admins can publish and manage content/resources
- [ ] Stripe integration for membership dues and event tickets
- [ ] Authentication via NextAuth.js / Auth.js

### Out of Scope

- Mobile app — web-first, responsive design sufficient for v1
- Real-time chat / messaging — not core to association value
- Multiple chapters/divisions — single org, one admin team for now
- OAuth social logins (Google, LinkedIn) — email/password via Auth.js for v1
- Automated email campaigns — manual comms or external tool for now

## Context

- **Organization type:** Professional association with 2000+ members
- **Event types:** Mix of conferences/summits, workshops/trainings, and casual meetups/networking
- **Event flow:** Admin creates event → members RSVP → attend
- **Membership model:** Free registration, members get perks (members-only events, content access, directory listing)
- **Payments:** Membership dues + event ticket sales via Stripe
- **Admin model:** One or two admins run everything — no complex role hierarchy needed yet
- **v1 scope:** Core member experience — signup, events, member content. Payments and full admin as fast-follow.
- **Scale consideration:** 2000+ members means pagination, search, and efficient queries are important from the start

## Constraints

- **Tech stack:** Next.js (App Router), MongoDB, NextAuth.js/Auth.js, Stripe
- **Architecture:** Monorepo if admin site is separated from member-facing site
- **Hosting:** Azure (Azure App Service or Azure Static Web Apps)
- **Payments:** Stripe for both membership dues and event tickets
- **Database:** MongoDB — document model fits events/members/content well

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Next.js for frontend + API | Full-stack framework, SSR for public pages, API routes for backend | — Pending |
| MongoDB over PostgreSQL | User preference, flexible schema for varied event types | — Pending |
| Monorepo (if needed) | Separate admin from public site without separate repos | — Pending |
| Auth.js over Clerk/Supabase | User preference, self-hosted auth, no vendor lock-in | — Pending |
| Azure hosting | User preference, enterprise-friendly for professional associations | — Pending |
| Stripe for payments | Industry standard, good API, handles both subscriptions and one-time | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-04-04 after initialization*
