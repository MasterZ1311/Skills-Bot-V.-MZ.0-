# 🚀 Workflow 03 — Launch a SaaS MVP

This workflow takes you from idea to your first paying customer. Designed for 1-3 person teams with limited time and budget.

---

## 🎯 Goal
Launch a SaaS product in 6 weeks and acquire your first 3 paying customers.

**Example:** College Events Platform — ₹999/month organizer subscriptions

---

## ⏱️ Timeline (6 Weeks)

| Week | Focus |
|---|---|
| 1 | Planning, architecture, infra setup |
| 2 | Core product (event creation + registration) |
| 3 | Payments + organizer dashboard |
| 4 | Marketing site + onboarding |
| 5 | Beta with 3 partner colleges |
| 6 | Launch + first customers |

---

## Week 1 — Foundation

### Step 1.1 — MVP Scoping
```
@brainstorming We're building a SaaS events platform for college organizers.
MVP goal: organizer can create an event and students can register.
Timeline: 6 weeks to first paying customer.

Help us scope ruthlessly:
- What are the 3 features absolutely necessary for someone to pay?
- What are the 5 features we want to build but must cut for MVP?
- What can be manual/hacky in Week 1 that we automate later?
- What's our "riskiest assumption" we must validate first?

Output: MVP scope (yes/no per feature) + risk assumptions ranked.
```

### Step 1.2 — Tech Stack Decision
```
@architecture Choose the simplest possible tech stack for our MVP:
Constraints: 2 developers, 6 weeks, < ₹5,000/month infra budget

Options to evaluate:
A. Next.js + Supabase + Vercel (1 deployment, Postgres + Auth + Storage included)
B. Next.js + PlanetScale + Clerk + Vercel (best-in-class each)
C. Next.js + Prisma + Neon + NextAuth + Vercel (most control)

Evaluate each on: setup time, operational complexity, cost at 500 users, lock-in risk.
Recommend the one that ships fastest for our 6-week deadline.
```

### Step 1.3 — Infra Setup
```
@supabase Set up Supabase for our SaaS platform:
- Project: events-platform (region: ap-south-1 for India)
- Database: initialize schema for users, events, registrations, subscriptions
- Auth: Google OAuth + Email/Password for organizers
- Storage: bucket for event images (public read, authenticated write)
- RLS: students see own data, organizers see their events, admins see all
- API keys: service_role for backend, anon for frontend

Also: configure custom SMTP (Resend) for transactional emails.
```

---

## Week 2 — Core Product

### Step 2.1 — Database Schema
```
@database-design Design the minimum schema for our SaaS MVP:
Keep it simple — we can add columns later.

Tables needed (MVP only):
1. profiles (extends Supabase auth.users): role, college, onboarding_complete
2. organizations (colleges/clubs): name, slug, plan, stripe_customer_id
3. events: title, description, date, capacity, price, status, org_id, image_url
4. registrations: event_id, user_id, status, stripe_payment_id
5. subscriptions: org_id, stripe_subscription_id, plan, current_period_end

RLS policies:
- Events: public can read published, org members can CRUD own
- Registrations: users see own, org members see for their events
```

### Step 2.2 — Event Creation Flow (Organizer)
```
@react-best-practices + @nextjs-app-router-patterns Build the event creation flow:
Route: /dashboard/events/new (organizer only)

Multi-step form:
Step 1: Basic info (title, description, category)
Step 2: Date, time, venue, capacity
Step 3: Ticket pricing (free or set price) + registration form fields
Step 4: Upload banner image (Supabase Storage)
Step 5: Review + Publish

Server Actions for each step (Next.js 15):
- Save draft on each step (don't lose progress)
- Validate with Zod server-side
- Upload image: presigned URL from Supabase
- Publish: set status='published', trigger SEO reindex

After publish: redirect to /dashboard/events/:id with "Share your event" prompt.
```

### Step 2.3 — Student Registration Flow
```
@backend-architect Build the registration flow:
Endpoint: POST /api/registrations

Steps:
1. Validate: event exists + published + has capacity + user not already registered
2. If free event: create registration directly (CONFIRMED)
3. If paid event: create Stripe PaymentIntent → return client_secret
4. Frontend: show Stripe payment form → complete payment → confirm via webhook
5. Webhook: payment_intent.succeeded → update registration to CONFIRMED → send email

Email: React Email template (confirmation with event details)
Ticket: include unique registration ID as QR code data
```

---

## Week 3 — Payments

### Step 3.1 — Stripe Integration
```
@stripe-integration Implement Stripe for our SaaS platform:

Part A: Paid ticket purchases
- PaymentIntent for per-event ticket sales
- Webhook: payment_intent.succeeded → confirm registration
- Refunds: when organizer cancels event or student cancels (if policy allows)

Part B: Organizer subscription (our revenue)
- Products: Starter (₹499/mo), Growth (₹1,499/mo)
- Checkout: Stripe hosted checkout for simplicity (no custom UI needed in MVP)
- Webhook: customer.subscription.created/updated/deleted → update org plan
- Portal: Stripe Customer Portal for organizers to manage subscription
- Trial: 14 days free, credit card not required for trial

Stripe Connect (defer to Week 8 post-MVP):
- Direct charges model: organizers receive ticket revenue directly
- For MVP: we collect all ticket revenue, pay organizers manually (acceptable for beta)
```

### Step 3.2 — Subscription Gating
```
@saas-multi-tenant Implement feature gating based on subscription plan:
Free plan limits:
- Max 3 published events
- Max 100 registrations per event
- Basic analytics (registration count only)

Growth plan unlocks:
- Unlimited events and registrations
- Advanced analytics (revenue, funnel, export)
- Custom registration form fields
- Priority email support

Implementation:
- Middleware: check org.plan before allowing feature access
- UI: "Upgrade" prompt when hitting limits (not hard error)
- Billing page: /dashboard/billing shows current plan + usage
- Gate check: useFeatureFlag() hook → shows upgrade CTA if on free plan
```

---

## Week 4 — Marketing Site

### Step 4.1 — Landing Page
```
@high-end-visual-design + @cro Design our marketing landing page:
URL: events.college.edu (or our chosen domain)

Above the fold (must see without scrolling):
- Headline: "Run Your College Events. Zero Hassle." (large, confident)
- Subheadline: "Create events, manage registrations, validate tickets — all in one place."
- CTA: "Start Free — No credit card required" → /signup
- Social proof: "Trusted by 50+ student organizations"
- Hero: screenshot/video of the dashboard (real product)

Below the fold:
- How it works: 3 steps (Create Event → Share Link → Track Registrations)
- Features: 3 key features with icons
- Testimonials: quotes from beta organizers (even if 3 people)
- Pricing: 3-column (Free, Growth, Scale) with feature comparison
- FAQ: top 5 questions
- CTA again: "Get started free"

SEO: title tag, meta description, Event schema for any events we feature.
```

### Step 4.2 — Onboarding Flow
```
@onboarding-cro Design the organizer onboarding flow:
Goal: organizer creates and publishes first event within 10 minutes of signup

Steps:
1. Sign up: email + Google, choose "I'm an event organizer"
2. Org setup: college name, your role (Student Union President, Club Lead, etc.)
3. Create first event: walk them through in-app (tutorial overlay)
4. Share page: show them their public event URL to share
5. Invite: "Invite a team member" (optional)
6. Done: "Your event is live! Here's what to do next →"

Email sequence (triggered from Brevo/Mailchimp):
- Day 0: "Welcome! Here's how to create your first event" (link to guide)
- Day 2 (if no event): "Need help? Here's a 2-min video"
- Day 5 (if no event): "We'll help you set up — book a quick call"
```

---

## Week 5 — Beta Launch

### Step 5.1 — Beta Partner Outreach
```
@linkedin-post-writer Write outreach messages for beta partners:
We want 3 college student unions to use our platform for free during beta.

Message to student union president on LinkedIn:
- Reference: specific event they recently organized (shows we did research)
- Problem: acknowledge the pain (WhatsApp coordination, Google Forms chaos)
- Offer: free use of our platform for their next 3 events
- Ask: 30-minute call this week
- Length: 5 sentences max

Subject line for email: "Free event management for [College Name] events"
```

### Step 5.2 — Feedback Collection
```
@customer-research Set up feedback collection during beta:

In-app:
- After first event published: "How was creating your event? 1-5 stars" (NPS-style)
- After first 10 registrations: "What would make this better?" (open text)
- Exit intent: if organizer inactive 5 days: "What's stopping you?" (multiple choice)

Weekly call:
- Schedule 30-min call with each beta organizer
- Questions to ask:
  1. Walk me through the last time you used the platform
  2. What was the most frustrating moment?
  3. What did you expect to happen that didn't?
  4. If you could wave a magic wand, what would you change?
  5. Would you pay ₹999/month? What would make it a clear yes?
```

---

## Week 6 — Launch & First Revenue

### Step 6.1 — Conversion to Paid
```
@churn-prevention + @pricing-strategy Convert beta users to paying customers:
After 2 weeks of free beta use:

Email sequence (convert to paid):
Email 1 (Day 14): "Your beta period ends in 7 days"
- Value recap: X events created, Y registrations, ₹Z in tickets sold
- "Here's what's changing: you'll need a paid plan to continue"
- CTA: "Choose your plan →"

Email 2 (Day 18): "3 days left — here's what you'll lose"
- Feature list they've been using
- Pricing: "₹999/month — less than a pizza per week"
- CTA: "Upgrade now →"

Email 3 (Day 20): "Last chance — tomorrow your events go private"
- Urgency (real — events will be hidden)
- CTA: "Upgrade in 2 minutes →"

Phone: call each organizer personally before deadline.
Goal: 3 of 5 beta users → paying (60% conversion = great for B2B).
```

### Step 6.2 — Public Launch
```
@launch-strategy Plan our public launch:
Channels for launch day:
1. Product Hunt: launch at 12:01am PST, coordinate upvotes from network
2. LinkedIn: founder post about the problem + solution journey
3. Twitter/X: thread: "We built X in 6 weeks, here's what we learned"
4. IndieHackers: "Show IH" post (good for SaaS founders audience)
5. College WhatsApp groups: direct message 20 student union groups
6. SEO: college event pages start ranking (plant this in Week 1)

Launch day checklist:
□ Error monitoring on (Sentry)
□ On-call: founder available all day
□ Stripe in live mode (not test)
□ Support chat active (Crisp or Intercom)
□ Analytics: PostHog live dashboard open
□ Social: scheduled posts queued
```

---

## Post-Launch — Growth Loop

### Step GrowthL — Sustainable Growth Engine
```
@growth-engine Design our growth loop post-launch:
Our viral moment: when a student registers for an event →
they receive a beautiful QR ticket →
they share "I'm going to [Event]!" on Instagram →
their classmates see it → click → discover our platform

Amplify this loop:
1. Make tickets beautiful (shareable image, not just functional QR)
2. Add "Powered by College Events" on ticket (free marketing)
3. Referral: organizer refers another organizer → 1 month free
4. SEO: every event creates a public page → [College] events rank on Google

Paid CAC target (for Scale phase):
- LinkedIn ads targeting "Student Union President": ₹3,000 CAC
- LTV: ₹1,499 × 18 months = ₹26,982
- LTV:CAC = 9:1 (excellent)
```

---

## 📊 SaaS Metrics Dashboard

Track these from Day 1:
```
@kpi-dashboard-design Build the SaaS metrics dashboard:
Daily metrics (check every morning):
- New signups (organizer accounts created)
- Trials started
- MRR (sum of active subscriptions)
- Events created today
- Registrations processed today

Weekly metrics (review every Monday):
- Trial → Paid conversion rate (target: > 20%)
- Monthly Churn Rate (target: < 5%)
- Net Revenue Retention (target: > 100%)
- New logos (new paying customers)
- Support tickets opened

Monthly metrics (board/investor level):
- MRR growth rate
- CAC (cost to acquire a customer)
- LTV (lifetime value)
- LTV:CAC ratio (target: > 3:1)
- ARR run rate
```
