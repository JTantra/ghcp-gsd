# Association Events Platform

## What This Is

A website for a professional association to help prospective and current members discover upcoming events and let members RSVP through a gated account flow. It also includes an internal staff area so association administrators can manage events, RSVP lists, and member records in one place.

## Core Value

Members can quickly find relevant association events and RSVP without friction.

## Current Milestone: v1.0 Initial Release

**Goal:** Launch the first web release for the association events platform.

**Target features:**
- Public visitors can browse upcoming association events.
- Members can sign in and RSVP for events.
- Staff can create, edit, and publish events.
- Staff can manage RSVP lists.
- Staff can manage member records, including membership status.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Visitors can browse upcoming association events publicly.
- [ ] Members can sign in and RSVP for events.
- [ ] Staff can create, edit, and publish events.
- [ ] Staff can manage RSVP lists for each event.
- [ ] Staff can manage member records, including membership status.

### Out of Scope

- Paid memberships and dues — not part of the first release.
- Paid event ticketing — keep the initial event flow focused on RSVP, not checkout.
- Community features like forums, messaging, or networking — not core to the initial event workflow.
- Native mobile apps — the first release is web-only.

## Context

- The site is for a professional association rather than a general public events business.
- The first release is primarily about event discovery and RSVP, not broad community engagement.
- Public visitors should be able to browse events, but only members should be able to RSVP.
- Staff need an internal workflow for creating events, reviewing RSVPs, and managing member records.
- Member management in v1 is limited to basic profile data and membership status.
- This is a brand new process rather than a replacement for an existing platform.

## Constraints

- **Platform**: Web application — the first release ships as a website.
- **Access model**: Public browsing with member-only RSVP — event visibility stays open while RSVP actions remain gated.
- **Scope**: No payments in v1 — dues and ticketing are deferred so the core event flow ships first.
- **Member management**: Basic records and status only — no full CRM or community directory in the initial release.

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Event discovery and RSVP is the primary v1 value | This is the main user-facing outcome that defines success | — Pending |
| Public event browsing with member-only RSVP | The association needs discoverability without opening RSVP to everyone | — Pending |
| Staff admin tools ship in v1 | Staff need to manage events, RSVPs, and members in the same product | — Pending |
| Member management starts simple | Basic profiles and status support the event workflow without turning v1 into a CRM | — Pending |
| The first release is web-only | A website is sufficient for the initial launch and avoids mobile app scope | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? -> Move to Out of Scope with reason
2. Requirements validated? -> Move to Validated with phase reference
3. New requirements emerged? -> Add to Active
4. Decisions to log? -> Add to Key Decisions
5. "What This Is" still accurate? -> Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check -> still the right priority?
3. Audit Out of Scope -> reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-04-04 after milestone v1.0 start*