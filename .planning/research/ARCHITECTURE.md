# Architecture Patterns

**Domain:** Association event discovery and RSVP platform
**Researched:** 2026-04-04

## Recommended Architecture

Use a modular monolith: one web application, one relational database, and strict internal module boundaries.

Suggested route grouping:

```text
app/
  (public)/events/
  (member)/my-events/
  (member)/events/[slug]/rsvp/
  (staff)/admin/events/
  (staff)/admin/events/[id]/rsvps/
  (staff)/admin/members/

modules/
  access/
  membership/
  events/
  rsvps/
  admin/
  notifications/
  audit/
```

## Component Boundaries

| Component | Responsibility | Communicates With |
|-----------|---------------|-------------------|
| Public Events | Published event list/detail and search visibility | Events read model, cache/revalidation |
| Access Control | Authentication, session lookup, role checks | Membership, admin, RSVP flows |
| Membership | Profile and membership status | Access control, RSVP eligibility |
| Events Domain | Event draft/publish lifecycle and metadata | Public browse, RSVP, admin |
| RSVP Domain | Create/update/cancel RSVP and capacity rules | Membership, events, notifications |
| Staff Admin | Operational CRUD UIs | Events, RSVP, membership, audit |
| Notifications | Confirmation and reminder delivery | RSVP and admin changes |
| Audit | Staff mutation visibility | Staff admin workflows |

## Data Flow

### Public Browse
1. Browser requests published event pages.
2. Server component loads published-event data.
3. Response is cached or revalidated.
4. Staff publish/unpublish invalidates public views.

### Member RSVP
1. Member submits RSVP from a protected page.
2. Server checks session, membership status, and event state.
3. Database transaction writes RSVP state with uniqueness protection.
4. Member-facing views are revalidated.
5. Confirmation communication is queued asynchronously.

### Staff Admin Change
1. Staff submits event/member/RSVP update.
2. Server verifies staff role and validates payload.
3. Domain mutation writes data and audit entry.
4. Public or member caches are refreshed as needed.

## Patterns To Follow

- Route-segment by audience: public, member, staff.
- Use server components for read-heavy pages.
- Use server actions for first-party form mutations.
- Enforce authorization on the server, not only in navigation.
- Keep membership eligibility in persisted data, not only session claims.
- Add unique constraints such as one RSVP per member per event.
- Keep email and external notifications out of database transactions.

## Anti-Patterns To Avoid

- Separate frontend and backend stacks for v1.
- Client-only authorization checks.
- Generic catch-all admin APIs that bypass domain rules.
- Static or cached personalized member pages.
- Public and staff event logic tangled in the same query layer.

## Suggested Build Order

1. Core foundation: users, members, events, RSVPs, roles, route segmentation.
2. Staff event management: create, edit, publish, unpublish events.
3. Public event browsing: published event list and event detail pages.
4. Member RSVP: sign-in, eligibility checks, RSVP create/cancel, basic "my events" visibility if included.
5. Staff operations and hardening: RSVP rosters, member management, audit and notification follow-up.

## Confidence

- Overall: Medium-high
- Architecture segmentation, auth placement, and relational data flow are high confidence.
- Exact module names and build order are opinionated but well aligned with the project scope.

## Sources

- https://nextjs.org/docs/app/getting-started/project-structure
- https://nextjs.org/docs/app/getting-started/fetching-data
- https://nextjs.org/docs/app/guides/caching-without-cache-components
- https://authjs.dev/guides/role-based-access-control
- https://www.prisma.io/docs/orm/prisma-schema/overview
- https://www.prisma.io/docs/orm/prisma-client/queries/transactions
- https://martinfowler.com/bliki/MonolithFirst.html
# Architecture Research

**Domain:** Professional Association Management  
**Researched:** 2026-04-04  
**Confidence:** HIGH

## Standard Architecture

### System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         AZURE                                   │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │              Azure App Service / Container App           │   │
│  │                                                          │   │
│  │  ┌────────────────────────────────────────────────────┐  │   │
│  │  │           Next.js App (Single Deployment)          │  │   │
│  │  │                                                    │  │   │
│  │  │  ┌─────────────┐  ┌─────────────┐  ┌──────────┐  │  │   │
│  │  │  │ (public)    │  │ (admin)     │  │ (auth)   │  │  │   │
│  │  │  │ Route Group │  │ Route Group │  │ Route Grp│  │  │   │
│  │  │  │             │  │             │  │          │  │  │   │
│  │  │  │ Home        │  │ Dashboard   │  │ Sign-in  │  │  │   │
│  │  │  │ Events      │  │ Members     │  │ Magic    │  │  │   │
│  │  │  │ Directory   │  │ Events Mgmt │  │ Link     │  │  │   │
│  │  │  │ Content     │  │ Content Mgmt│  │ Callback │  │  │   │
│  │  │  │ Profile     │  │ Payments    │  │          │  │  │   │
│  │  │  └─────────────┘  └─────────────┘  └──────────┘  │  │   │
│  │  │                                                    │  │   │
│  │  │  ┌────────────────────────────────────────────┐   │  │   │
│  │  │  │           API Route Handlers               │   │  │   │
│  │  │  │  /api/members  /api/events  /api/payments  │   │  │   │
│  │  │  │  /api/content  /api/webhooks/stripe        │   │  │   │
│  │  │  └────────────────────────────────────────────┘   │  │   │
│  │  │                                                    │  │   │
│  │  │  ┌──────────┐  ┌──────────┐  ┌───────────────┐   │  │   │
│  │  │  │ Auth.js  │  │ Stripe   │  │ Server        │   │  │   │
│  │  │  │ (v5)     │  │ SDK      │  │ Actions       │   │  │   │
│  │  │  └──────────┘  └──────────┘  └───────────────┘   │  │   │
│  │  └────────────────────────────────────────────────────┘  │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
│                              ▼                                   │
│  ┌────────────────────┐  ┌──────────────────────────────────┐   │
│  │  Azure Blob Storage│  │         MongoDB Atlas            │   │
│  │  (uploads/assets)  │  │   (primary operational store)    │   │
│  └────────────────────┘  └──────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
                    │                        │
                    ▼                        ▼
          ┌──────────────┐         ┌──────────────────┐
          │ Stripe API   │         │ Email Service    │
          │ (Payments)   │         │ (Resend/SES)     │
          └──────────────┘         └──────────────────┘
```

### Architecture Decision: Single App with Route Groups (NOT Monorepo)

**Recommendation: Single Next.js app with route groups** over a monorepo with separate admin/public apps.

**Why:**
- 2000 members + 1-2 admins = low traffic. No scaling reason to separate.
- Shared auth (Auth.js), shared DB models, shared components — splitting creates duplication.
- Route groups `(public)`, `(admin)`, `(auth)` provide clean separation without deployment complexity.
- Middleware handles access control in one place.
- Single deployment to Azure = simpler CI/CD, lower cost.
- A monorepo (Turborepo + separate `apps/public` + `apps/admin`) is warranted when admin needs independent scaling, different deployment schedules, or separate teams. None apply here.

**If you later need separation:** Extract admin into a separate app within a Turborepo monorepo, sharing `packages/db` and `packages/ui`. The route-group structure makes this migration straightforward since the boundaries are already clean.

### Component Responsibilities

| Component | Responsibility | Implementation |
|-----------|----------------|----------------|
| **Public Site** `(public)` | Member-facing pages: events, directory, content, profile | Next.js route group with Server Components, public layouts |
| **Admin Panel** `(admin)` | CRUD for events, members, content, payment dashboard | Next.js route group, admin layout with role guard |
| **Auth** `(auth)` | Sign-in, registration, email verification, password reset | Auth.js with Credentials provider, MongoDB adapter, password hashing, transactional email |
| **API Routes** `/api/*` | REST endpoints for client-side mutations, webhooks | Next.js Route Handlers |
| **Server Actions** | Form submissions, RSVP, profile updates | Colocated `actions.ts` in feature directories |
| **Database Layer** `lib/db/` | All MongoDB access via Mongoose models | Singleton connection, model definitions, query helpers |
| **Stripe Integration** `lib/stripe/` | Membership checkout, event tickets, webhook processing | Stripe SDK, webhook signature verification |
| **Email Service** `lib/email/` | Email verification, password reset, event reminders, membership confirmations | Resend SDK (or Azure Communication Services) |
| **Middleware** | Route protection, role-based access, redirects | Next.js middleware.ts at root |

## Recommended Project Structure

```
src/
├── app/
│   ├── (public)/                    # Member-facing site
│   │   ├── layout.tsx               # Public layout (nav, footer)
│   │   ├── page.tsx                 # Home / landing
│   │   ├── events/
│   │   │   ├── page.tsx             # Events listing (paginated)
│   │   │   └── [slug]/
│   │   │       └── page.tsx         # Event detail + RSVP
│   │   ├── directory/
│   │   │   └── page.tsx             # Member directory (search + pagination)
│   │   ├── content/
│   │   │   ├── page.tsx             # Articles/resources listing
│   │   │   └── [slug]/
│   │   │       └── page.tsx         # Article detail (members-only gated)
│   │   └── profile/
│   │       ├── page.tsx             # View/edit own profile
│   │       └── membership/
│   │           └── page.tsx         # Membership status, upgrade, billing portal
│   │
│   ├── (admin)/                     # Admin panel
│   │   ├── layout.tsx               # Admin layout (sidebar, breadcrumbs)
│   │   ├── admin/
│   │   │   ├── page.tsx             # Dashboard (stats, recent activity)
│   │   │   ├── members/
│   │   │   │   ├── page.tsx         # Members list (search, filter, export)
│   │   │   │   └── [id]/
│   │   │   │       └── page.tsx     # Member detail/edit
│   │   │   ├── events/
│   │   │   │   ├── page.tsx         # Events management
│   │   │   │   ├── new/
│   │   │   │   │   └── page.tsx     # Create event
│   │   │   │   └── [id]/
│   │   │   │       └── page.tsx     # Edit event + view RSVPs
│   │   │   ├── content/
│   │   │   │   ├── page.tsx         # Content management
│   │   │   │   └── [id]/
│   │   │   │       └── page.tsx     # Edit content
│   │   │   └── payments/
│   │   │       └── page.tsx         # Payment history, revenue reports
│   │   └── _components/             # Admin-only components
│   │       ├── sidebar.tsx
│   │       └── data-table.tsx
│   │
│   ├── (auth)/                      # Auth flows
│   │   ├── layout.tsx               # Minimal centered layout
│   │   ├── sign-in/
│   │   │   └── page.tsx             # Email/password sign-in
│   │   ├── register/
│   │   │   └── page.tsx             # Registration form
│   │   ├── forgot-password/
│   │   │   └── page.tsx             # Request password reset email
│   │   ├── reset-password/
│   │   │   └── [token]/
│   │   │       └── page.tsx         # Set new password
│   │   └── verify/
│   │       └── page.tsx             # Email verification status
│   │
│   ├── api/
│   │   ├── auth/[...nextauth]/
│   │   │   └── route.ts             # Auth.js catch-all handler
│   │   ├── members/
│   │   │   └── route.ts             # Member search/list API (for client-side)
│   │   ├── events/
│   │   │   ├── route.ts             # Events listing API
│   │   │   └── [id]/
│   │   │       └── rsvp/
│   │   │           └── route.ts     # RSVP toggle
│   │   ├── webhooks/
│   │   │   └── stripe/
│   │   │       └── route.ts         # Stripe webhook handler
│   │   └── admin/                   # Admin-only APIs
│   │       ├── members/
│   │       │   └── route.ts         # Admin member CRUD
│   │       └── export/
│   │           └── route.ts         # CSV export
│   │
│   ├── layout.tsx                   # Root layout (providers, fonts)
│   └── not-found.tsx
│
├── lib/
│   ├── db/
│   │   ├── connection.ts            # MongoDB singleton connection
│   │   ├── models/
│   │   │   ├── user.ts              # User model (Auth.js compatible)
│   │   │   ├── member.ts            # Membership details (extends User)
│   │   │   ├── event.ts             # Event model
│   │   │   ├── rsvp.ts              # RSVP (join table: user + event)
│   │   │   ├── content.ts           # Articles, resources
│   │   │   └── payment.ts           # Payment records (Stripe mirror)
│   │   └── index.ts                 # Re-exports all models
│   ├── auth/
│   │   ├── config.ts                # Auth.js configuration
│   │   ├── actions.ts               # signIn/signOut server actions
│   │   └── guards.ts                # requireAuth(), requireAdmin() helpers
│   ├── stripe/
│   │   ├── client.ts                # Stripe SDK singleton
│   │   ├── checkout.ts              # Create checkout sessions
│   │   ├── webhooks.ts              # Webhook event processors
│   │   └── portal.ts                # Customer billing portal
│   ├── email/
│   │   ├── client.ts                # Email provider setup
│   │   └── templates/               # Email templates
│   │       ├── email-verification.tsx
│   │       ├── password-reset.tsx
│   │       ├── event-reminder.tsx
│   │       └── membership-welcome.tsx
│   └── utils/
│       ├── pagination.ts            # Cursor/offset pagination helpers
│       └── search.ts                # Text search query builders
│
├── components/
│   ├── ui/                          # Generic UI components (shadcn/ui)
│   ├── events/
│   │   ├── event-card.tsx
│   │   └── rsvp-button.tsx
│   ├── members/
│   │   ├── member-card.tsx
│   │   └── directory-search.tsx
│   └── layout/
│       ├── navbar.tsx
│       ├── footer.tsx
│       └── auth-button.tsx
│
├── middleware.ts                     # Route protection + redirects
├── auth.ts                          # Auth.js init (exports handlers, auth, signIn, signOut)
└── next.config.ts
```

## Data Models

### Entity Relationship Overview

```
┌──────────┐      ┌────────────┐       ┌──────────┐
│  User    │──1:1─│  Member    │──1:N──│ Payment  │
│ (auth)   │      │ (profile)  │       │ (stripe) │
└──────────┘      └────────────┘       └──────────┘
     │                  │
     │                  │ N:M (via RSVP)
     │                  │
     │            ┌──────────┐
     │            │   RSVP   │
     │            └──────────┘
     │                  │
     │            ┌──────────┐       ┌───────────┐
     └────────────│  Event   │──N:1──│ EventType │
                  └──────────┘       └───────────┘
                        
┌───────────┐
│  Content  │  (standalone, access controlled by membership tier)
└───────────┘

┌───────────┐
│  Session  │  (Auth.js managed session records)
└───────────┘

┌───────────────────┐
│PasswordResetToken │  (custom verification/reset token store)
└───────────────────┘
```

### Model Definitions

**User** (Auth.js compatible — required fields for MongoDB adapter)
```typescript
{
  _id: ObjectId,
  name: string,
  email: string,                    // unique, indexed
  emailVerified: Date | null,       // Auth.js field
  image: string | null,
  role: 'user' | 'admin',          // custom extension
  createdAt: Date,
  updatedAt: Date
}
// Indexes: { email: 1 } (unique)
```

**Member** (extends user with association-specific data)
```typescript
{
  _id: ObjectId,
  userId: ObjectId,                 // ref → User, unique
  tier: 'free' | 'standard' | 'premium',
  status: 'active' | 'expired' | 'cancelled',
  stripeCustomerId: string | null,
  stripeSubscriptionId: string | null,
  subscriptionExpiresAt: Date | null,
  profile: {
    bio: string,
    company: string,
    jobTitle: string,
    phone: string,
    linkedIn: string,
    website: string,
    showInDirectory: boolean        // opt-in to public directory
  },
  joinedAt: Date,
  updatedAt: Date
}
// Indexes: { userId: 1 } (unique), { tier: 1, status: 1 }, 
//          { 'profile.showInDirectory': 1 }, { stripeCustomerId: 1 }
// Text index: { 'profile.company': 'text', 'profile.bio': 'text' }
```

**Event**
```typescript
{
  _id: ObjectId,
  title: string,
  slug: string,                     // unique, URL-friendly
  description: string,
  type: 'conference' | 'workshop' | 'meetup' | 'webinar',
  status: 'draft' | 'published' | 'cancelled' | 'completed',
  startDate: Date,
  endDate: Date,
  location: {
    type: 'in-person' | 'virtual' | 'hybrid',
    venue: string | null,
    address: string | null,
    virtualLink: string | null
  },
  capacity: number | null,          // null = unlimited
  rsvpCount: number,                // denormalized counter
  membersOnly: boolean,
  requiresTicket: boolean,
  ticketPrice: number | null,       // cents
  stripeProductId: string | null,
  stripePriceId: string | null,
  imageUrl: string | null,
  createdBy: ObjectId,              // ref → User
  createdAt: Date,
  updatedAt: Date
}
// Indexes: { slug: 1 } (unique), { status: 1, startDate: -1 },
//          { type: 1 }, { startDate: 1 }
```

**RSVP** (join collection — explicit rather than embedded to support querying from both sides)
```typescript
{
  _id: ObjectId,
  userId: ObjectId,                 // ref → User
  eventId: ObjectId,                // ref → Event
  status: 'registered' | 'waitlisted' | 'cancelled' | 'attended',
  ticketPurchased: boolean,
  stripePaymentIntentId: string | null,
  registeredAt: Date,
  updatedAt: Date
}
// Indexes: { userId: 1, eventId: 1 } (unique compound),
//          { eventId: 1, status: 1 }
```

**Content**
```typescript
{
  _id: ObjectId,
  title: string,
  slug: string,                     // unique
  body: string,                     // markdown or rich text
  excerpt: string,
  category: string,
  tags: string[],
  access: 'public' | 'members' | 'premium',
  status: 'draft' | 'published',
  publishedAt: Date | null,
  author: ObjectId,                 // ref → User
  createdAt: Date,
  updatedAt: Date
}
// Indexes: { slug: 1 } (unique), { status: 1, publishedAt: -1 },
//          { access: 1 }, { tags: 1 }
// Text index: { title: 'text', body: 'text', tags: 'text' }
```

**Payment** (Stripe event mirror — source of truth is Stripe, this is for querying)
```typescript
{
  _id: ObjectId,
  userId: ObjectId,                 // ref → User
  stripePaymentIntentId: string,    // unique
  stripeInvoiceId: string | null,
  type: 'membership' | 'event_ticket',
  amount: number,                   // cents
  currency: string,
  status: 'pending' | 'succeeded' | 'failed' | 'refunded',
  metadata: {
    eventId: ObjectId | null,
    tier: string | null
  },
  createdAt: Date
}
// Indexes: { userId: 1, createdAt: -1 }, { stripePaymentIntentId: 1 } (unique),
//          { type: 1, status: 1 }
```

### Schema Design Rationale

| Decision | Rationale |
|----------|-----------|
| **Separate User + Member** | User model must match Auth.js adapter expectations. Member data is association-specific and shouldn't pollute the auth layer. 1:1 link via `userId`. |
| **RSVP as separate collection** | Embedding RSVPs in Event documents would hit the 16MB limit for popular events and make per-user queries expensive. Separate collection with compound index supports both directions efficiently. |
| **Denormalized `rsvpCount` on Event** | Avoids counting RSVPs on every event listing. Increment/decrement via `$inc` on RSVP create/cancel. |
| **Payment as mirror collection** | Stripe is source of truth. Local collection enables admin reports and user payment history without Stripe API calls on every page load. Updated via webhooks. |
| **Text indexes for search** | MongoDB text indexes support the directory search and content search at 2000-member scale. No need for Elasticsearch/Algolia initially. |

## Data Flow

### 1. Public Page Load (e.g., Events Listing)

```
Browser → Next.js Server Component → Mongoose query → MongoDB
                    ↓
            Rendered HTML (streamed via RSC)
                    ↓
                 Browser
```

- Server Components fetch data directly via `lib/db` — no API route needed.
- Paginated with cursor-based pagination for stable ordering.
- Cached with Next.js `revalidate` for public pages (60-300s).

### 2. Authenticated Action (e.g., RSVP to Event)

```
Browser → Server Action (actions.ts)
              ↓
         auth() → verify session (Auth.js)
              ↓
         Mongoose: create RSVP
         Mongoose: Event.$inc({ rsvpCount: 1 })
              ↓
         revalidatePath('/events/[slug]')
              ↓
         Return result → Browser UI updates
```

- Server Actions for all state-changing operations.
- Auth check at the start of every action via `auth()` helper.
- `revalidatePath()` ensures fresh data on next page visit.

### 3. Membership Payment Flow

```
Browser → Server Action: createCheckoutSession()
              ↓
         Stripe SDK → Create Checkout Session
              ↓
         Redirect → Stripe Checkout page
              ↓
         User pays → Stripe sends webhook
              ↓
         POST /api/webhooks/stripe
              ↓
         Verify signature → Process event
              ↓
         Update Member: tier, status, stripeSubscriptionId
         Create Payment record
              ↓
         Stripe redirects user → /profile/membership?success=true
```

- Webhook is the source of truth for payment completion (never trust client-side redirect alone).
- Webhook handler is idempotent (check if payment already processed).

### 4. Admin CRUD (e.g., Create Event)

```
Browser (Admin) → Server Action: createEvent(formData)
                      ↓
                 requireAdmin() → verify session + role
                      ↓
                 Validate input (zod schema)
                      ↓
                 Mongoose: Event.create(data)
                      ↓
                 revalidatePath('/events')
                 revalidatePath('/admin/events')
                      ↓
                 redirect('/admin/events/[new-id]')
```

### 5. Middleware Route Protection

```
Every request → middleware.ts
    ↓
    Check path:
    /admin/*   → auth() → require role === 'admin' → else redirect /sign-in
    /profile/* → auth() → require authenticated → else redirect /sign-in
    /api/admin/* → auth() → require role === 'admin' → else 401
    /api/webhooks/* → pass through (Stripe verifies via signature)
    everything else → pass through
```

## API Design

### Server Actions (Primary — for forms and mutations)

| Action | Location | Auth | Description |
|--------|----------|------|-------------|
| `rsvpToEvent(eventId)` | `app/(public)/events/[slug]/actions.ts` | User | Register/cancel RSVP |
| `updateProfile(data)` | `app/(public)/profile/actions.ts` | User | Update member profile |
| `createCheckout(tier)` | `app/(public)/profile/membership/actions.ts` | User | Create Stripe session |
| `createEvent(data)` | `app/(admin)/admin/events/actions.ts` | Admin | Create new event |
| `updateEvent(id, data)` | `app/(admin)/admin/events/actions.ts` | Admin | Edit event |
| `deleteEvent(id)` | `app/(admin)/admin/events/actions.ts` | Admin | Soft-delete event |
| `updateMember(id, data)` | `app/(admin)/admin/members/actions.ts` | Admin | Admin edit member |

### Route Handlers (For client-side fetching, webhooks, exports)

| Endpoint | Method | Auth | Purpose |
|----------|--------|------|---------|
| `GET /api/members` | GET | User | Directory search (paginated, filtered) |
| `GET /api/events` | GET | Public | Events list for client-side filtering |
| `POST /api/events/[id]/rsvp` | POST | User | RSVP (alternative to Server Action) |
| `POST /api/webhooks/stripe` | POST | Stripe sig | Process payment events |
| `GET /api/admin/export/members` | GET | Admin | CSV member export |

### Design Principle

**Prefer Server Actions over API Routes.** Server Actions are type-safe, colocated with the UI, and don't require building a separate API client. Use Route Handlers only when:
1. Client-side JS needs to fetch (e.g., infinite scroll, search-as-you-type)
2. External services call you (webhooks)
3. Non-browser consumers need access (exports, integrations)

## Deployment Architecture (Azure)

### Recommended: Azure App Service (Linux) with Docker

| Component | Azure Service | Rationale |
|-----------|---------------|-----------|
| **Next.js App** | App Service (B1/B2) or Container App | SSR support, custom domain, TLS, scales to 0 on Container Apps |
| **Database** | MongoDB Atlas (primary) or Cosmos DB (MongoDB API) | Atlas gives cleaner MongoDB compatibility; Cosmos DB is an Azure-first alternative when policy or networking requirements dominate |
| **File Storage** | Azure Blob Storage | Event images, member avatars, exported CSVs |
| **Email** | Resend or Azure Communication Services | Email verification, password reset, event reminders |
| **CI/CD** | GitHub Actions | Build → Docker image → push to Azure Container Registry → deploy |

### Why App Service over Static Web Apps

Azure Static Web Apps has limited SSR support for Next.js (API routes restrictions, cold starts, limited middleware). App Service or Container Apps give full Node.js runtime with no Next.js feature restrictions.

### Environment Variables

```
# Auth
AUTH_SECRET=...
AUTH_URL=https://yourassociation.org

# Database
MONGODB_URI=mongodb+srv://...

# Stripe
STRIPE_SECRET_KEY=sk_...
STRIPE_WEBHOOK_SECRET=whsec_...
STRIPE_PUBLISHABLE_KEY=pk_...

# Email
RESEND_API_KEY=re_...
EMAIL_FROM=noreply@yourassociation.org

# Azure
AZURE_STORAGE_CONNECTION_STRING=...
```

## Anti-Patterns to Avoid

| Anti-Pattern | Why Bad | Do Instead |
|-------------|---------|------------|
| **Premature monorepo split** | 2000 members, 1-2 admins. Separate deployments = 2x infra cost, shared-nothing auth complexity. | Single app, route groups. Split later if needed. |
| **Embedding RSVPs in Event docs** | Popular events = massive documents, 16MB limit risk, slow reads. | Separate RSVP collection with compound indexes. |
| **Calling own API routes from Server Components** | Unnecessary network hop. Server Components can access DB directly. | Import `lib/db` in Server Components. Use Route Handlers only for client-side JS. |
| **Storing Stripe state as source of truth** | Local DB can drift. Webhook failures lose data. | Stripe is source of truth. Local Payment collection is a queryable mirror. Always reconcile via webhooks. |
| **Using `useEffect` for data fetching** | Server Components make client-side fetching unnecessary for initial loads. | Fetch in Server Components. Use client components only for interactivity (search, RSVP button). |
| **Skipping text indexes for search** | At 2000 members, MongoDB text search is sufficient. Elasticsearch adds operational complexity for no gain. | Use MongoDB text indexes. Revisit at 10K+ or complex search needs. |

## Build Order

Dependencies flow downward. Each layer depends on the one above it being complete.

```
Phase order (each depends on previous):

1. PROJECT FOUNDATION
   ├── Next.js skeleton (App Router, route groups, layouts)
   ├── MongoDB connection + Mongoose setup
     ├── Auth.js credentials flow + MongoDB adapter + verification/reset flows
   └── Middleware (route protection)
   
   WHY FIRST: Everything depends on auth + DB + routing.

2. MEMBER PROFILE + DIRECTORY
   ├── Member model + profile CRUD
   ├── Directory page (search, pagination)
   └── Profile page (view/edit)
   
   WHY SECOND: Members are the core entity. Events and content reference them.

3. EVENTS SYSTEM
   ├── Event model + CRUD (admin)
   ├── Public event listing + detail pages
   ├── RSVP system (with capacity checks)
   └── Event filtering/search
   
   WHY THIRD: Core value proposition. Depends on auth + members.

4. STRIPE + MEMBERSHIP
   ├── Stripe integration (checkout, webhooks)
   ├── Membership tiers + subscription lifecycle
   ├── Event ticket purchases
   └── Admin payment dashboard
   
   WHY FOURTH: Depends on members + events existing. Complex integration that benefits from stable foundation.

5. CONTENT + ACCESS CONTROL
   ├── Content model + CRUD (admin)
   ├── Members-only content gating
   └── Public content pages
   
   WHY FIFTH: Lower priority than events. Access control depends on membership tiers.

6. ADMIN DASHBOARD + POLISH
   ├── Admin dashboard with stats
   ├── Member management (admin view)
   ├── CSV exports
   └── Email notifications (event reminders, welcome emails)
   
   WHY LAST: Operational tooling. Depends on all data existing.

7. DEPLOYMENT + INFRASTRUCTURE
   ├── Docker containerization
   ├── Azure App Service / Container App deploy
   ├── Azure Blob Storage integration
   ├── GitHub Actions CI/CD
   └── Custom domain + TLS
   
   NOTE: Can run in parallel with Phase 3+ using Vercel/local for dev.
```

### Build Order Rationale

The ordering follows **dependency chains**:
- Auth is foundational — nothing works without login.
- Members are the core entity — events, content, and payments all reference members.
- Events are the primary user activity — the most-used feature should be built early to get feedback.
- Stripe is complex and benefits from a stable data model — don't integrate payments until member and event schemas are proven.
- Content is important but lower interaction frequency — can come after the event system.
- Admin tooling and deployment are ongoing concerns but don't block member-facing features.

### Scalability Considerations

| Concern | At 2K members (now) | At 10K members | At 50K+ members |
|---------|---------------------|----------------|-----------------|
| **Database** | Single MongoDB instance, text indexes | Add read replicas, compound indexes | Shard by region or tier |
| **Search** | MongoDB text search | Still fine with proper indexes | Consider Atlas Search or Elasticsearch |
| **Hosting** | App Service B1 ($13/mo) | App Service B2/S1 | Container Apps with autoscaling |
| **File storage** | Azure Blob (minimal) | CDN in front of Blob | Same, with image optimization |
| **Email** | Resend free tier | Resend paid or SES | SES with queuing |
| **Architecture** | Single app, route groups | Same | Consider splitting admin to separate app |

---
*Architecture research for: Professional Association Management*  
*Researched: 2026-04-04*  
*Sources: Next.js docs (Context7), Auth.js docs (Context7), Mongoose docs (Context7), community architecture patterns, Azure deployment guides*
