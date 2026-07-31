# 🚀 SaaS Startup Operator — Skills Guide

SaaS startup operators wear every hat — building the product, growing users, processing payments, handling support, and running business operations. This guide covers MVP launch, payments, analytics, CRM, support, and business ops.

---

## 🗺️ Skill Map

| Concern | Top Skills |
|---|---|
| MVP Launch | `@saas-mvp-launcher`, `@micro-saas-launcher`, `@saas-multi-tenant`, `@app-builder` |
| Payments | `@stripe-integration`, `@payment-integration`, `@paypal-integration`, `@plaid-fintech` |
| Analytics | `@analytics`, `@analytics-tracking`, `@posthog-automation`, `@amplitude-automation` |
| Support & CRM | `@customer-support`, `@hubspot-automation`, `@intercom-automation`, `@zendesk-automation` |
| Project Management | `@jira-automation`, `@linear-automation`, `@notion-automation`, `@github-automation` |
| Marketing | `@growth-engine`, `@email-sequence`, `@seo-content-writer`, `@linkedin-post-writer` |
| Business Ops | `@business-analyst`, `@legal-advisor`, `@hr-pro` |
| Churn | `@churn-prevention` |

---

## 🏗️ MVP Launch

### `@saas-mvp-launcher`
Launch a SaaS product from zero to paying customers.
```
@saas-mvp-launcher Launch our college events SaaS platform:
Target customers: college student unions and event organizing clubs
Problem: manual event management via WhatsApp + Google Forms
Our solution: structured platform for event creation, registration, and analytics

MVP scope (6 weeks):
Week 1-2: Core — event creation, registration form, email confirmation
Week 3: Payments — Stripe integration for paid events
Week 4: Organizer dashboard — registration list, revenue, QR scanner
Week 5: Marketing site + pricing page
Week 6: Beta launch with 3 partner colleges

Non-MVP (defer):
- Mobile app (web first)
- Advanced analytics
- API for third-party integrations

Success metric: 3 paying organizers after Week 6.
What's the critical path? What should we cut if behind schedule?
```

### `@saas-multi-tenant`
Design multi-tenancy for a SaaS platform.
```
@saas-multi-tenant Design multi-tenant architecture for our events platform:
Tenants: colleges (each college is an isolated tenant)

Tenancy model options:
A. Shared database, shared schema: tenant_id on every table
B. Shared database, separate schemas: one schema per tenant
C. Separate database per tenant: maximum isolation, maximum cost

For our use case (500 colleges, moderate data per tenant):
Recommend option and explain tradeoffs.

For chosen option:
- How to implement data isolation (never mix tenant data)
- How to add new tenant (onboarding flow)
- How to run migrations without downtime across all tenants
- How to handle tenant-specific customization (their brand colors)
- Pricing: how to track usage per tenant for billing
```

### `@micro-saas-launcher`
Solo founder micro-SaaS playbook.
```
@micro-saas-launcher We're a 2-person team launching our events platform.
Resource constraints: $5,000 budget, 3 months runway.

Lean launch playbook:
- Tech: Next.js + Supabase + Vercel (minimize infra ops)
- Payments: Stripe (no custom billing until $10K MRR)
- Support: Crisp chat (free tier handles < 500 users)
- Analytics: PostHog (generous free tier)
- Marketing: SEO + LinkedIn organic only until revenue

First 30 days goal: 1 paying customer (even at ₹999/month)
How to find them: cold outreach to 10 college student union presidents via LinkedIn
Value proposition: "runs your event in 5 minutes, not 5 hours"

What to say in the first message? Write the cold outreach script.
```

---

## 💳 Payments

### `@stripe-integration`
Full Stripe implementation: one-time, subscriptions, Connect.
```
@stripe-integration Implement Stripe for our events platform:

Use case 1: Paid event tickets (one-time payments)
- Create PaymentIntent when student starts registration
- Capture on confirmation (not immediate — allow review step)
- Webhook: payment_intent.succeeded → create registration + send ticket
- Refund flow: registration cancelled → refund PaymentIntent

Use case 2: Organizer subscriptions (recurring)
- Plans: Starter (₹499/mo), Growth (₹1,499/mo), Scale (₹3,999/mo)
- Customer Portal: let organizers manage subscription themselves
- Webhook: customer.subscription.updated → update plan in DB
- Trial: 14-day free trial, no card required

Use case 3: Stripe Connect (marketplace payments)
- Organizer receives ticket revenue directly
- Platform takes 3% fee
- Express Connect accounts for organizers
- Stripe handles payouts, taxes, compliance

Show: Node.js SDK code for each use case + webhook handler.
```

### `@payment-integration` / `@paypal-integration`
PayPal and other payment methods.
```
@paypal-integration Add PayPal as a payment option for events:
Some students prefer PayPal over card payments.

Integration:
- PayPal Smart Payment Buttons (frontend SDK)
- Server-side order creation: POST /api/paypal/create-order
- Capture on approval: POST /api/paypal/capture-order
- Webhook: PAYMENT.CAPTURE.COMPLETED → create registration
- Sandbox testing: PayPal developer sandbox accounts

UX consideration: show PayPal button alongside Stripe card form.
Which should be primary? A/B test recommendation.
```

### `@plaid-fintech`
Plaid for bank account verification and ACH payments.
```
@plaid-fintech Add ACH bank transfer option for large institutional payments:
Use case: colleges paying annual subscription via bank transfer (not card)

Plaid integration:
- Link: student/admin connects their bank account
- Auth: retrieve account/routing number
- Via Stripe: use Plaid token to create Stripe ACH payment method
- Transaction: schedule ACH payment for annual subscription
- Confirm: ACH takes 3-5 business days — handle pending state

When to offer: B2B sales to college institutions > ₹50,000/year deals.
```

---

## 📊 Analytics

### `@analytics-tracking`
Set up a complete analytics stack.
```
@analytics-tracking Set up analytics for our SaaS events platform:

Product analytics: PostHog
- Track: page views, feature usage, user paths
- Funnels: signup → first event created → first registration → payment
- Session recording: understand where organizers get confused

Business analytics: custom dashboard
- MRR (Monthly Recurring Revenue)
- MRR growth rate (MoM)
- Churn rate (monthly)
- LTV (Customer Lifetime Value)
- CAC (Customer Acquisition Cost)
- Net Revenue Retention (NRR)

Marketing analytics: UTM tracking
- Track every link we share with UTM parameters
- Source attribution: where do signups come from?

Data warehouse: connect PostHog → Google BigQuery (weekly export)
```

---

## 💬 Customer Support

### `@customer-support`
Set up an efficient customer support system.
```
@customer-support Design our customer support system for 3 support tiers:

Tier 1 — Self-service (handles 60% of issues):
- Help center: Intercom Articles or Notion
- Pages: "How to create an event", "Where is my ticket?", "Refund policy"
- In-app: contextual help tooltips on complex features
- Chatbot: answer common questions automatically

Tier 2 — Live chat (handles 30% of issues):
- Intercom Inbox: shared inbox for 2-person team
- Response SLA: < 4 hours business hours
- Canned responses for common issues
- Escalation: tag for founders when unusual

Tier 3 — Emergency (handles 10% of issues):
- Phone: Google Voice number for event-day emergencies
- Organizers only, not students
- On-call: one founder reachable during large events

Support metrics: CSAT, FRT (first response time), resolution time.
```

### `@intercom-automation`
Automate customer support and engagement with Intercom.
```
@intercom-automation Set up Intercom for our events platform:

Outbound messages (proactive):
1. New organizer, Day 1: "Create your first event in 5 minutes →" (in-app)
2. New organizer, Day 3 (if no event created): "Need help getting started?" (email)
3. Event created but no registrations after 48h: "Promote your event →" (in-app)
4. Approaching free tier limit (3rd event): "Upgrade to get unlimited events" (in-app)

Inbound routing:
- Message contains "refund": route to billing queue
- Message contains "error", "bug", "not working": route to technical queue
- All others: general support queue

Bot:
- Greet: "Hi! What can I help you with? [My event isn't showing] [Refund question] [Other]"
- Deflect with help articles before routing to human
```

---

## 🗂️ Project Management

### `@linear-automation`
Linear for engineering team task management.
```
@linear-automation Set up Linear for our engineering team:

Teams: Engineering, Design, Marketing
Cycles: 2-week sprints
States: Backlog → Todo → In Progress → In Review → Done

Issue templates:
- Feature: user story, acceptance criteria, design link, API contract
- Bug: steps to reproduce, expected vs actual, severity, affected users
- Tech debt: description, impact, estimated effort

Automation:
- PR opened → linked issue moves to In Review
- PR merged → linked issue moves to Done
- Issue unassigned for 2 days → notify engineering lead
- Sprint start → auto-create "Sprint Kickoff" template issue

Integrations: GitHub (PR links), Slack (notifications), Sentry (error → issue)
```

### `@notion-automation`
Notion as team knowledge base and operations hub.
```
@notion-automation Set up Notion for our startup operations:

Databases:
1. Projects: track all active initiatives (status, owner, timeline)
2. Meetings: log all meetings with decisions and action items
3. Customers: CRM — each customer with status, MRR, last contact
4. Ideas: feature requests and growth ideas (vote to prioritize)

Templates:
- Weekly team standup template
- Customer discovery interview notes
- Product spec (PRD template)
- Retrospective

Automations:
- New customer added → notify team in Slack
- Project status changes to "Done" → auto-archive
- Meeting notes → extract action items (using AI block)
```

---

## 📋 Business Operations

### `@business-analyst`
Business analysis, unit economics, and financial modeling.
```
@business-analyst Build a unit economics model for our events platform:

Revenue per customer:
- ARPU (Average Revenue Per User — organizer): ₹1,499/month (Growth plan average)
- Add: per-registration revenue: ₹2.50 × 200 registrations/month = ₹500
- Total ARPU: ₹1,999/month

Cost per customer:
- Infrastructure: ₹200/month (compute, DB, storage per college)
- Support: 30 min/month × ₹1,000/hr = ₹500
- Payment processing: 2% × ₹1,999 = ₹40
- Total CoGS: ₹740/month
- Gross Margin: (₹1,999 - ₹740) / ₹1,999 = 63%

Payback period:
- CAC: ₹5,000 per customer (LinkedIn outreach + demo time)
- Months to payback: ₹5,000 / ₹1,259 contribution margin = 4 months

LTV:
- Average customer lifetime: 24 months (college contract cycle)
- LTV: ₹1,259 × 24 = ₹30,216
- LTV:CAC = 6:1 (healthy > 3:1)

What's our path to ₹10 Lakh MRR? Show customer count milestone.
```

### `@churn-prevention`
Identify at-risk customers and prevent churn.
```
@churn-prevention Build a churn prevention system for our organizer customers:

Early warning signals (in order of importance):
1. No events created in 30 days (most predictive)
2. Registration count declining month-over-month
3. No login in 14 days
4. Support ticket opened + closed without resolution
5. Pricing page visited (evaluating downgrade/cancel)

Intervention playbook:
- Signal 1 (no events 30d): personal email from founder + offer 1-hour help call
- Signal 2 (declining registrations): share best practices + offer marketing tips
- Signal 3 (no login 14d): in-app "We miss you" + remind of upcoming features
- Signals 4+5: proactive call from account manager

Measurement: track intervention → churn prevented rate. 
Goal: reduce monthly churn from 5% to 2%.
```

### `@legal-advisor`
Legal considerations for SaaS startups.
```
@legal-advisor Review legal requirements for our SaaS events platform in India:

Documents needed:
1. Terms of Service: user obligations, platform liability, IP ownership, payment terms
2. Privacy Policy: PDPB (India's data protection law) compliance, what data collected, retention
3. Refund Policy: clear terms for ticket refunds, organizer refund obligations
4. Organizer Agreement: platform's right to remove events, fee structure, payout terms

Compliance:
- GST: do we need to register for GST? At what revenue threshold?
- Payment gateway: RBI regulations for payment aggregators?
- Data localization: student data must be stored in India?
- Age: minors using platform — consent requirements?

Entity structure: should we register as Pvt Ltd immediately or operate as sole proprietor until ₹X revenue?
```

---

## 🔗 Complete SaaS Startup Operator Chain

```
1️⃣  @saas-mvp-launcher
    "Define MVP scope, critical path, 6-week launch plan"

2️⃣  @saas-multi-tenant
    "Design multi-tenancy architecture for college isolation"

3️⃣  @stripe-integration
    "Set up: paid tickets (PaymentIntent) + organizer subscriptions"

4️⃣  @analytics-tracking
    "PostHog for product, custom dashboard for business metrics"

5️⃣  @email-sequence
    "Onboarding email sequence: organizer activation in 7 days"

6️⃣  @intercom-automation
    "Automated support: self-serve help center + proactive messages"

7️⃣  @growth-engine
    "Design acquisition: SEO + LinkedIn + referral loop"

8️⃣  @churn-prevention
    "Early warning signals + intervention playbook"

9️⃣  @business-analyst
    "Unit economics: ARPU, CAC, LTV, payback, path to ₹10L MRR"

🔟  @legal-advisor
    "Legal: ToS, Privacy Policy, GST, entity structure"
```
