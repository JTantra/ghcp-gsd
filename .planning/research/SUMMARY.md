# Research Summary: Association Events Platform

**Domain:** Professional association event discovery, RSVP, and staff operations
**Researched:** 2026-04-04
**Overall confidence:** Medium-high

## Executive Summary

The research points to a straightforward initial product shape: a single web application that combines public event discovery, member-only RSVP, and staff administration on top of a relational data model. The main risks are not novel technical architecture problems; they are operational ones such as registration friction, stale membership data, duplicate RSVPs, and weak staff workflows.

For this scope, the strongest approach is a modular monolith built with Next.js, PostgreSQL, Prisma, and Auth.js. Public event pages should be server-rendered and cacheable. Member and staff mutations should remain server-enforced, with membership status treated as domain data rather than a UI hint.

Feature-wise, the true v1 table stakes are public event browsing, member sign-in before RSVP, immediate RSVP confirmation, staff event publishing, RSVP roster visibility, and basic member status management. Payments, deeper community tooling, and full CRM behavior are all clear later-phase work and should stay out of the first milestone.

## Key Findings

**Stack:** Single Next.js app, PostgreSQL, Prisma, Auth.js, Zod, and staff-focused table/form utilities are the strongest fit for v1.

**Features:** Public event browsing, member-only RSVP, staff event CRUD, RSVP lists, and basic member record management form the MVP boundary.

**Architecture:** Split by audience and workflow: public pages, member flows, and staff admin. Keep all domain rules server-side and relational.

**Critical pitfall:** Registration friction and disconnected member data create immediate trust and operations problems, even in an otherwise small release.

## Implications for Roadmap

Suggested phase structure:

1. **Foundation and access control**
   - Establish users, member status, roles, core event and RSVP models, and route segmentation.
   - This phase exists because member-only RSVP and staff admin both depend on trusted access control.

2. **Staff event management**
   - Deliver staff event create/edit/publish lifecycle first so the system can produce usable event inventory.
   - This also solves the common leak risk between draft and public events.

3. **Public event discovery**
   - Deliver published event list and detail pages once event publishing exists.
   - Public browsing is valuable independently and validates the event content model.

4. **Member RSVP**
   - Deliver sign-in, membership-aware RSVP, and confirmation flow once public event viewing and access control are in place.
   - This is where duplicate prevention, server-side eligibility checks, and confirmation quality matter most.

5. **Staff operations and member management**
   - Deliver RSVP roster tools, member status edits, and operational hardening such as audit visibility and follow-up workflows.
   - This phase absorbs the staff-side pitfalls that emerge after core member flows exist.

## Research Flags for Planning

- **High-confidence phases with standard patterns:** foundation/access, public event discovery.
- **Phases that need careful planning:** member RSVP and staff operations, because eligibility rules, duplicate prevention, and operational tables are easy to under-spec.

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | High | Strong alignment between current framework/auth/database guidance and product shape. |
| Features | Medium-high | Table stakes are consistent across association event operations guidance. |
| Architecture | Medium-high | Recommended structure is opinionated but well supported for this scope. |
| Pitfalls | Medium-high | Operational failure patterns repeat across current event-registration guidance. |

## Gaps to Address

- Exact event metadata needed in v1 still needs product-level scoping.
- Decide whether members need a "my events" area in the first milestone or immediately after.
- Decide whether RSVP cancellation/change is part of the first member workflow or deferred.

## Sources

- https://nextjs.org/docs/app
- https://authjs.dev/guides/role-based-access-control
- https://www.prisma.io/docs
- https://internet4associations.com/guides/association-event-management/
- https://www.associationsonline.com/management/event-registration-without-the-headaches-how-modern-associations-handle-peak-season/
- https://readymembership.com/resource/why-traditional-event-platforms-fail-membership-organizations.html
