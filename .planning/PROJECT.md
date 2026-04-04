# Association Events Platform

## What This Is

A web application for a professional association to help prospective and current members discover upcoming events and let members RSVP through a gated account flow. It includes an internal admin area so association staff can manage events, member records, and membership status without relying on separate tools.

## Core Value

Members can quickly find relevant association events and RSVP without friction.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Visitors can browse upcoming association events publicly.
- [ ] Members can sign in and RSVP for events.
- [ ] Members can maintain a basic profile.
- [ ] Admins can create and manage workshops and meetups.
- [ ] Admins can manage member records and membership status.

### Out of Scope

- Paid memberships and dues — not part of v1.
- Paid event ticketing — keep the initial event flow simple.
- Members-only resource library — event experience is the priority.
- Member directory and networking features — defer until the event workflow is solid.
- Fully gated public site — public event discovery matters for awareness and conversion.

## Context

- The product is for a professional association with 2000+ members.
- The most important v1 outcome is smooth event discovery and RSVP for members.
- The main event types in scope are workshops and meetups.
- Public users should be able to browse events, but RSVP actions are member-only.
- The first release includes a built-in admin area rather than relying on external tools.
- Member management in v1 focuses on profile data and active or inactive membership status.

## Constraints

- **Tech stack**: Next.js web app — already preferred for the main application.
- **Authentication**: Magic link sign-in — reduce friction for members.
- **Scale**: 2000+ members — search, filtering, and list performance need to hold up early.
- **Scope**: No payments in v1 — dues and ticketing stay out until the core event flow works.

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Event discovery and RSVP is the primary v1 value | This is the main user-facing outcome that defines success | — Pending |
| Public event browsing with gated member RSVP | The association needs public visibility without opening member actions | — Pending |
| Built-in admin area in v1 | Staff need to manage events and member status in the same product | — Pending |
| Magic link authentication | Lower sign-in friction for members than password-first auth | — Pending |
| Next.js for the web application | A concrete framework choice already exists | — Pending |

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