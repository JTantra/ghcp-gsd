# Feature Landscape

**Domain:** Association event discovery and RSVP platform
**Researched:** 2026-04-04

## Table Stakes

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| Public event list and detail pages | Visitors need to discover events without friction | Medium | Must support published/unpublished state cleanly. |
| Member sign-in before RSVP | Member-only registration is core to association value | Medium | Sign-in and eligibility are separate concerns. |
| Pre-populated member information in registration flows | Members should not re-enter data the association already has | Medium | Requires member-profile integration. |
| Mobile-friendly registration and RSVP flow | Registrations increasingly happen on phones | Medium | Especially important for event reminder and last-minute sign-up behavior. |
| Immediate confirmation after RSVP | Users expect certainty after registering | Low | On-screen confirmation plus email is the standard pattern. |
| Staff event create/edit/publish workflow | Staff need to manage inventory without engineering help | Medium | Draft/publish states reduce accidental public exposure. |
| Staff RSVP roster visibility | Staff need operational visibility on attendees | Medium | Filtering and export are common follow-on needs. |
| Member record and status management | RSVP gating depends on trusted membership status | Medium | Even basic v1 needs status awareness and edits. |

## Differentiators

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| Join-and-register flow | Converts interested visitors into members at the point of intent | High | Valuable later, but not required for first release. |
| My Events view for members | Reduces support burden and improves self-service | Medium | Good candidate soon after core RSVP flow. |
| Self-service RSVP changes/cancellations | Cuts staff workload during peak season | Medium | Worth considering once basic RSVP is stable. |
| Attendance history and renewal insight | Connects events to member retention outcomes | High | Better as post-v1 reporting or analytics work. |

## Anti-Features

| Anti-Feature | Why Avoid | What to Do Instead |
|--------------|-----------|-------------------|
| Paid ticketing and checkout | Adds pricing, refunds, and finance complexity beyond RSVP scope | Keep RSVP free in v1. |
| Broad community features | Forums, messaging, and networking dilute the release goal | Focus on event discovery and RSVP. |
| Full CRM behavior | Deep member lifecycle tooling expands scope too far | Keep member records basic and event-relevant. |
| Separate mobile app | Doubles delivery surface too early | Ship responsive web first. |
| Complex sponsor, exhibitor, or CE tracking workflows | Common in mature association event tools but too large for first release | Defer until core flows are proven. |

## Dependencies Between Features

- Staff event publishing must exist before public browsing has meaningful data.
- Membership-aware sign-in must exist before member-only RSVP can be enforced.
- Member status records must exist before RSVP eligibility can be checked reliably.
- RSVP confirmation depends on both event detail data and member identity.
- Staff RSVP management depends on the RSVP flow existing first.

## MVP Recommendation

Prioritize for v1:
1. Public browsing of published events.
2. Member sign-in and membership-aware RSVP.
3. Staff event CRUD and publish workflow.
4. Staff RSVP list management.
5. Basic member record management with membership status.

Defer to later milestones:
- Join-and-register combined flow.
- Self-service RSVP changes beyond the core action.
- Analytics, attendance insight, and retention reporting.
- Payments, sponsorship tooling, and CE administration.

## Confidence

- Overall: Medium-high
- Table-stakes confidence is high because the same patterns appear across modern association and event operations guidance.
- Differentiator and anti-feature guidance is medium-high because some items depend on organization size and maturity, but they are clear scope boundaries for an initial release.

## Sources

- https://internet4associations.com/guides/association-event-management/
- https://www.associationsonline.com/management/event-registration-without-the-headaches-how-modern-associations-handle-peak-season/
- https://readymembership.com/resource/why-traditional-event-platforms-fail-membership-organizations.html
# Feature Research

**Domain:** Professional Association Management  
**Researched:** 2026-04-04  
**Confidence:** HIGH  

> Sources: i4a Small Association Software Guide (2026), NextBee 12 Key AMS Features (2025), AssociationsOnline AMS Guide (2026), Protech Top 21 AMS Platforms (2026), competitive landscape analysis across 10+ AMS vendors. Cross-referenced against project constraints: ~2000 members, 1-2 admins, Auth.js-based credentials auth, Stripe payments.

## Feature Landscape

### Table Stakes (Users Expect These)

These are features that members of a professional association will expect from day one. Missing any of these creates friction that drives members away.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| **Member registration & profiles** | Users need to create accounts and manage their info. Every AMS has this. | Low | Use clear email/password registration with email verification, password reset, and profile basics such as name, company, role, and join date. |
| **Event listing & RSVP** | Events are the primary engagement driver for associations. Members expect to browse and sign up easily. | Medium | Conferences, workshops, meetups. Need event detail pages, RSVP/registration, capacity tracking, waitlists. |
| **Event calendar view** | Members need to see what's coming up at a glance. Standard in every competitor. | Low | Monthly/list view. Filter by event type. |
| **Member-only content gating** | Membership perks must be visible and exclusive. Otherwise why pay dues? | Low | Gate certain pages/resources behind membership status check. |
| **Member directory** | Networking is a core value proposition of professional associations. Members expect to find peers. | Medium | Searchable by name, company, role. Members opt-in to visibility. Privacy controls matter. |
| **Email notifications** | Transactional emails (signup confirmation, RSVP confirmation, event reminders) are baseline expectations. | Low | Use transactional email service (Resend, SendGrid, Postmark). Not marketing automation — just essential notifications. |
| **Membership dues payment** | If you charge dues, payment must be frictionless. Failed payment = lost member. | Medium | Stripe checkout, recurring billing, grace periods. Members expect self-service payment management. |
| **Event ticket purchase** | Paid events need seamless checkout. Members expect to pay and get instant confirmation. | Medium | Stripe checkout for one-time payments. Member vs non-member pricing tiers. |
| **Member self-service portal** | Members expect to update their own profile, view their membership status, see event history. | Low-Medium | Profile editing, membership status, event history, payment history. Reduces admin burden dramatically. |
| **Admin dashboard** | 1-2 admins need to manage members, events, and content without touching code. | Medium | Member list with search/filter, event CRUD, content management, basic analytics (member count, event attendance). |
| **Responsive/mobile-friendly design** | 60%+ of association member engagement happens on mobile (TechSoup 2025). | Low | Not a native app — responsive web design. Must work well on phones for event check-in and browsing. |
| **Basic reporting** | Admins need visibility into membership trends, event attendance, and revenue. | Low-Medium | Member count over time, renewal rates, event attendance, revenue by month. Simple charts, not enterprise BI. |

### Differentiators (Competitive Advantage)

These features aren't expected but create meaningful value. They separate a modern, purpose-built platform from clunky off-the-shelf AMS software.

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| **Magic link sign-in (optional enhancement)** | Reduces password friction for returning members, especially on mobile. | Low | Keep this as a later enhancement if the team wants passwordless login after the credentials flow is stable. |
| **Clean, modern UX** | AMS platforms are notoriously ugly and clunky (this is a top complaint across review sites). A fast, attractive member experience stands out immediately. | Medium | The biggest complaint about existing AMS platforms is poor UI/UX. A well-designed custom site has massive advantage here. |
| **Event waitlist with auto-promotion** | When a spot opens, next person automatically gets invited. Most AMS tools require manual management. | Low-Medium | Auto-notify when spot opens. Time-limited claim window. Significantly reduces admin work for popular events. |
| **Member engagement scoring** | Identify at-risk members before they churn. Track event attendance, content access, login frequency. | Medium | Simple score: attended events + accessed content + login recency. Helps admins focus retention efforts. v2+ feature. |
| **Smart event recommendations** | Surface relevant events based on member interests/history. Most AMS platforms blast everything to everyone. | Medium | Based on past attendance, stated interests, role. Simple tag matching, not ML. v2+ feature. |
| **Public event pages with SEO** | Many AMS platforms hide events behind login walls. Public event pages drive organic discovery and new member acquisition. | Low | Public event detail pages that rank in search. CTA to register (free) or join (member). Growth channel. |
| **Automated renewal reminders** | Multi-touch reminder sequence before and after due date. Most small associations do this manually. | Low-Medium | Email sequence: 30 days before, 7 days before, due date, 7 days after (grace), final notice. Stripe handles retry logic for failed payments. |
| **QR code event check-in** | Fast check-in at physical events without printed lists or manual lookup. | Low | Generate QR on RSVP confirmation. Admin scans at door. Simple but impressive in practice. |

### Anti-Features (Commonly Requested, Often Problematic)

These are features that seem logical but create disproportionate complexity, maintenance burden, or poor member experience for a ~2000-member single association.

| Feature | Why Requested | Why Problematic | Alternative |
|---------|---------------|-----------------|-------------|
| **Built-in forum / discussion board** | "Members should connect!" | Forums require moderation, die without critical mass, and compete with tools members already use (Slack, Discord, LinkedIn). Dead forums signal an unhealthy community. | Link to a Discord/Slack community. Let purpose-built tools handle real-time chat. |
| **Native mobile app** | "Everyone's on mobile!" | Massive development and maintenance burden (2 platforms). App Store approval friction. Push notification complexity. For 2000 members, the ROI is terrible. | Responsive web app with PWA capabilities (add to home screen, offline basics). Covers 95% of mobile needs at 5% of the cost. |
| **Built-in CMS / blog** | "We need to publish content!" | Building a CMS is a multi-month project. Competing with WordPress, Ghost, etc. on content editing UX is unwinnable. | Use a headless CMS (Sanity, Contentful, or even Markdown/MDX in repo) for member content. Or just use a CMS for the marketing site and gate member resources. |
| **Complex workflow automation** | "Automate everything!" | Workflow builders are enterprise features that 1-2 admins won't use. They add complexity to every feature and create hidden failure modes. | Simple, hardcoded automations: renewal reminders, RSVP confirmations, welcome emails. Cover 90% of needs with 10% of the complexity. |
| **Chapter / branch management** | "Other AMS platforms have it!" | Single association with no chapters. This is enterprise AMS feature creep. Adds data model complexity, permission layering, and admin UI sprawl for zero value. | Not needed. If sub-groups emerge, use event categories or tags. |
| **Certification / credentialing** | "Track member certifications!" | Complex workflow: applications, reviews, renewals, CE credits, compliance reporting. It's practically its own product. | If needed later, integrate with a dedicated credentialing platform. Don't build this. |
| **Complex reporting / BI dashboards** | "We need analytics!" | Enterprise BI tools are overkill for 2000 members. Building chart builders, custom report designers, and pivot tables is months of work for data that fits in a spreadsheet. | Simple, pre-built reports (member count, event attendance, revenue). CSV export for anything custom — admins can use Excel/Sheets. |
| **Multi-language support** | "What about international members?" | i18n touches every string in the app. Massive ongoing maintenance. For a single professional association, the working language is known. | Build in English. If truly needed later, it's a v3+ consideration. |
| **Social features (feeds, likes, comments)** | "Make it like LinkedIn!" | Building social features well requires years of iteration. Half-baked social features feel worse than no social features. Members already have LinkedIn. | Link social profiles in member directory. Cross-post events to LinkedIn. Don't compete. |
| **AI chatbot / AI features** | "AI is everywhere in 2026!" | AI features need training data, ongoing tuning, and add unpredictable costs. At 2000 members, the FAQ page handles it. Premature AI is expensive complexity theater. | Good search, clear FAQs, responsive admin support. If member questions become repetitive at scale, revisit. |

## Feature Dependencies

```
Member Registration ──────────────────────────────┐
  │                                                │
  ├── Member Profile ─────────────────────┐        │
  │     │                                 │        │
  │     ├── Member Directory              │        │
  │     │                                 │        │
  │     └── Member Self-Service Portal    │        │
  │           │                           │        │
  │           └── Payment History View    │        │
  │                                       │        │
  ├── Email/Password Auth ────────────────┤        │
  │                                       │        │
  ├── Membership Status ──────────────────┤        │
  │     │                                 │        │
  │     ├── Membership Dues Payment ──────┤        │
  │     │     │                           │        │
  │     │     └── Renewal Reminders       │        │
  │     │                                 │        │
  │     └── Content Gating                │        │
  │                                       │        │
  └── Event RSVP ─────────────────────────┘        │
        │                                          │
        ├── Event Ticket Purchase                  │
        │                                          │
        ├── Event Waitlist                         │
        │                                          │
        ├── QR Code Check-in                       │
        │                                          │
        └── Event Calendar View ───────────────────┘

Admin Dashboard (depends on all member-facing features existing)
  ├── Member Management (CRUD, search, filter)
  ├── Event Management (CRUD, attendee lists)
  ├── Content Management (member-only resources)
  └── Basic Reporting (aggregates from all above)
```

**Key dependency insight:** Member registration + auth is the foundation everything builds on. Events and membership status are parallel tracks that converge at "member perks" (gated content, member pricing). Payments layer on top of both membership and events.

## MVP Definition

### Launch With (v1)

Core member experience — a member can register, browse events, RSVP, and access member content.

| Feature | Rationale |
|---------|-----------|
| Member registration with email/password auth | Foundation for everything. Clear for admins and members, and compatible with Auth.js plus standard recovery flows. |
| Member profiles (view/edit own) | Self-service reduces admin burden from day one. |
| Event listing, detail pages, RSVP | Events are the #1 engagement driver. This is why members visit. |
| Event calendar view | Quick overview of upcoming events. Low effort, high utility. |
| Member-only content/resources | Makes membership tangibly valuable. Simple gate on existing content. |
| Member directory | Core networking value. Searchable, with privacy opt-in. |
| Email notifications (transactional) | RSVP confirmations, event reminders, welcome email. Essential communication. |
| Responsive design | Non-negotiable in 2026. Not a feature — a requirement. |
| Basic admin: manage members | Admins need to see/search/edit members. |
| Basic admin: manage events | Admins need to create/edit/publish events. |
| Public event pages | SEO-friendly entry point for member acquisition. |

### Add After Validation (v1.x)

Monetization layer + admin efficiency — validate that members are engaged before adding payment complexity.

| Feature | Rationale |
|---------|-----------|
| Stripe membership dues payment | Monetization. Requires subscription billing, webhook handling, grace periods. |
| Event ticket purchase (Stripe) | Paid events. Member vs non-member pricing tiers. |
| Automated renewal reminders | Reduces churn. Multi-touch email sequence. |
| Event waitlist with auto-promotion | Popular events need this. Low complexity, high perceived value. |
| Full admin dashboard | Complete CRUD for all entities, basic reporting, content management. |
| Payment history (member view) | Self-service billing. Reduces "did my payment go through?" support. |
| Basic reporting (admin) | Member counts, event attendance, revenue trends. Pre-built views, not custom. |
| CSV export | Escape valve for any reporting need. Admins use Excel for edge cases. |

### Future Consideration (v2+)

| Feature | Rationale |
|---------|-----------|
| QR code event check-in | Nice for large conferences. Not needed until events are running smoothly. |
| Member engagement scoring | Requires enough data (events, logins, content access) to be meaningful. |
| Smart event recommendations | Needs event history data. Tag-based matching. |
| Marketing email campaigns | Beyond transactional. Segment-based newsletters, announcements. Consider integration with Mailchimp/ConvertKit vs building. |
| PWA capabilities | Add-to-home-screen, offline event schedule. Progressive enhancement. |

## Feature Prioritization Matrix

| Feature | User Value | Implementation Cost | Priority |
|---------|------------|---------------------|----------|
| Member registration + credentials auth | Critical | Low | **P0 — Launch** |
| Event listing + RSVP | Critical | Medium | **P0 — Launch** |
| Member profiles | High | Low | **P0 — Launch** |
| Member-only content gating | High | Low | **P0 — Launch** |
| Member directory | High | Medium | **P0 — Launch** |
| Event calendar view | Medium | Low | **P0 — Launch** |
| Transactional email | High | Low | **P0 — Launch** |
| Public event pages (SEO) | Medium | Low | **P0 — Launch** |
| Basic admin (members + events) | Critical (admin) | Medium | **P0 — Launch** |
| Stripe membership dues | Critical | Medium-High | **P1 — Fast follow** |
| Stripe event tickets | High | Medium | **P1 — Fast follow** |
| Renewal reminders | High | Low-Medium | **P1 — Fast follow** |
| Event waitlist | Medium | Low | **P1 — Fast follow** |
| Full admin dashboard | High (admin) | Medium | **P1 — Fast follow** |
| Basic reporting | Medium (admin) | Low-Medium | **P1 — Fast follow** |
| CSV export | Medium (admin) | Low | **P1 — Fast follow** |
| QR check-in | Medium | Low | **P2 — v2+** |
| Engagement scoring | Medium | Medium | **P2 — v2+** |
| Event recommendations | Low-Medium | Medium | **P2 — v2+** |
| Marketing emails | Medium | Medium-High | **P2 — v2+** |

## Key Insights for Roadmap

1. **Events are the killer feature.** For professional associations, event attendance is the #1 driver of member engagement and the #1 reason members join. Build events first and build them well.

2. **Payments are a fast-follow, not launch blocker.** You can launch with free registration + free events + member content gating. Add Stripe once the core experience is validated. This also keeps v1 scope manageable.

3. **The UX IS the differentiator.** Every source confirms: AMS platforms have terrible UX. A clean, modern, fast member experience is the single biggest competitive advantage over off-the-shelf tools. Don't clutter it with features.

4. **Admin experience matters but it's second priority.** 1-2 admins can tolerate a rougher admin UI at launch. Polish the member experience first. Admin improvements follow based on real pain points.

5. **Don't build what you can integrate.** Email marketing (Mailchimp), community chat (Discord/Slack), CMS (headless), and payment (Stripe) are all better as integrations than custom builds. Build the glue, not the components.

---
*Feature research for: Professional Association Management*  
*Researched: 2026-04-04*  
*Sources: i4a.com (2026), NextBee (2025), AssociationsOnline (2026), Protech Associates (2026), competitive analysis of 10+ AMS platforms*
