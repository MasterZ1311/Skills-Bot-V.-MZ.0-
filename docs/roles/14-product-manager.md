# 📋 Product Manager — Skills Guide

Product managers define what to build, why it matters, and how to measure success. This guide covers requirements writing, analytics, user research, roadmapping, launch strategy, and monetization — all powered by `@skill-name` invocations.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| Planning & PRDs | `@product-manager`, `@to-prd`, `@before-you-build`, `@planning-and-task-breakdown` |
| Ideation | `@product-inventor`, `@brainstorming`, `@product-design` |
| User Research | `@customer-research`, `@jobs-to-be-done-analyst`, `@competitor-analysis` |
| Analytics | `@analytics-product`, `@posthog-automation`, `@amplitude-automation`, `@kpi-dashboard-design` |
| Roadmap & ADRs | `@architecture-decision-records`, `@documentation-and-adrs` |
| Launch | `@launch-strategy`, `@shipping-and-launch` |
| Monetization | `@monetization`, `@pricing-strategy`, `@usage-based-pricing`, `@free-tier-strategy` |
| Market Research | `@market-sizing-analysis`, `@startup-analyst` |
| Business Ops | `@business-analyst`, `@legal-advisor` |

---

## 📝 Requirements & PRDs

### `@to-prd`
Convert feature ideas or requirements into structured Product Requirements Documents.
```
@to-prd Convert these requirements into a full PRD:

Feature: Waitlist system for sold-out events
Problem: 40% of popular events sell out within minutes. 
         Students who miss registration feel frustrated with no recourse.
Goal: Reduce student FOMO, automatically fill cancellation spots,
      improve organizer revenue (near 100% occupancy).

PRD structure needed:
- Executive summary (2 sentences)
- Problem statement with user research backing
- Goals and non-goals
- User stories (student, organizer, admin perspectives)
- Detailed acceptance criteria
- Edge cases and failure modes
- Success metrics (primary + guardrail metrics)
- Open questions
- Out of scope
- Timeline estimate
```

### `@product-manager` / `@product-manager-toolkit`
General PM decision-making frameworks and toolkits.
```
@product-manager We have three feature requests competing for next sprint:
1. Waitlist system (requested by 50 students, high dev complexity)
2. Event calendar sync (requested by 20 students, low complexity)
3. Organizer analytics dashboard (strategic for growth, medium complexity)

Help me prioritize using:
- RICE scoring (Reach, Impact, Confidence, Effort)
- Dependencies and risks for each
- Which aligns best with our Q3 OKR: increase organizer retention by 20%
```

### `@before-you-build`
Pre-build risk and assumption checklist.
```
@before-you-build Before we build the waitlist feature, run through the checklist:
- Have we validated that students would actually join a waitlist (not just move on)?
- Do we have user research data or is this assumed?
- What's the technical risk? (Race conditions, notification delivery failures)
- What's the business risk? (Organizers lose accurate headcount)
- What's the legal risk? (Commitment without guarantee of entry?)
- What's our rollback plan if it creates more problems than it solves?
- Is there a simpler version we should build first?
```

### `@planning-and-task-breakdown`
Break down a feature into executable engineering tasks.
```
@planning-and-task-breakdown Break down the waitlist feature into engineering tasks:
PRD summary: [paste 3-line summary]

Expected breakdown:
- Backend: DB schema, service, API endpoints, capacity release trigger
- Frontend: UI components, waitlist position display, join/leave flow
- Notifications: email/push when spot opens, auto-cancel if not confirmed
- Testing: unit, integration, E2E, load test for concurrent joins
- Analytics: instrument join, notify, convert events

Format as: task title, description, estimated story points, dependencies.
```

---

## 🔍 User Research

### `@jobs-to-be-done-analyst`
Framework for understanding what users truly need.
```
@jobs-to-be-done-analyst Analyze the JTBD for college event registration:

Core job: "When I'm a bored student on a Tuesday evening, 
I want to find something interesting happening on campus tonight, 
so I feel connected to my college community."

Decompose into:
- Functional job: what task are they completing?
- Emotional job: how do they want to feel?
- Social job: how do they want to be perceived?

Pain points in the current experience:
- Discovery: where do students find out about events?
- Registration friction: too many steps?
- Attendance barrier: forgetting, not finding venue?

What unmet needs create the biggest opportunity?
```

### `@customer-research`
Plan and synthesize user interviews and surveys.
```
@customer-research Plan a 5-interview user research session on event discovery:
Target users: college students who attended at least 1 event in the last month

Interview guide:
- Warm up: tell me about last event you attended. How did you find out?
- Discovery: walk me through how you decide which events to attend.
- Barriers: tell me about a time you wanted to go to an event but didn't.
- Current tools: what apps or methods do you use today?

After interviews, I'll synthesize:
- Key insight themes
- Jobs to be done
- Top 3 opportunities for our product
```

### `@competitor-analysis`
Structured competitive landscape analysis.
```
@competitor-analysis Competitive analysis for our college events platform:
Direct competitors: Eventbrite (for campus events), Luma, Meetup
Indirect: Facebook Events, Instagram, WhatsApp groups, college notice boards

For each, analyze:
- Target customer (who are they really for?)
- Pricing model
- Key features
- Strengths (what do they do well?)
- Weaknesses (where do they fall short for college use?)
- Market position

Conclusion: where is our defensible differentiation?
(Hint: none are built specifically for Indian college campuses)
```

### `@market-sizing-analysis`
TAM/SAM/SOM calculation for strategic planning.
```
@market-sizing-analysis Calculate the market opportunity for our events platform:
Geography: India
Target: college students (18-24) who attend campus events

Data points:
- India has ~40,000 colleges
- Average enrollment: 2,000 students
- Target initially: 500 "active" colleges (urban, English-medium)
- Monetization: ₹2 per registration + ₹999/month organizer subscription

Calculate: TAM, SAM (reachable in 3 years), SOM (Year 1 target)
Show assumptions clearly.
```

---

## 📊 Analytics & Metrics

### `@posthog-automation`
Set up PostHog for product analytics.
```
@posthog-automation Set up PostHog for our events platform:

Events to track:
- event_viewed: { event_id, source (search|home|share), user_id }
- registration_started: { event_id, event_price }
- registration_step_completed: { event_id, step (form|payment|confirm) }
- registration_completed: { event_id, revenue, payment_method }
- registration_abandoned: { event_id, last_step }
- ticket_viewed: { ticket_id, event_id }
- waitlist_joined: { event_id, position }

Funnels to create:
1. Discovery → Registration: event_viewed → registration_completed
2. Registration funnel: started → step1 → step2 → completed

Feature flags:
- new_registration_flow: A/B test for 20% of users
- waitlist_feature: gradual rollout (10% → 50% → 100%)
```

### `@amplitude-automation`
Amplitude for behavioral cohort analysis.
```
@amplitude-automation Set up Amplitude for cohort analysis:
- Cohort: "Power Users" — registered for 3+ events in 30 days
- Cohort: "At Risk" — active last month, no activity this month
- Behavioral analysis: what do power users do in first 7 days?
- Retention curve: Day 1, 7, 14, 30 retention by registration source
- Revenue correlation: does attending more events → more paid events?
```

### `@kpi-dashboard-design`
Design the PM metrics dashboard.
```
@kpi-dashboard-design Design our PM dashboard for weekly review:

North Star Metric: Monthly Active Registrations (MAR)
  → Target: 1,000 MAR by Q3 end

Supporting metrics (leading indicators):
1. Weekly new user signups (target: 200/week)
2. Event discovery → registration conversion (target: > 20%)
3. 30-day retention (target: > 40%)
4. Organizer satisfaction score (NPS target: > 50)
5. Revenue (gross ticket value processed)

Guardrail metrics (must not regress):
- Registration error rate (< 1%)
- Support ticket volume

Format: weekly snapshot + 8-week trend + target status (on track / at risk / behind)
```

---

## 🚀 Launch Strategy

### `@launch-strategy`
Plan a phased product launch.
```
@launch-strategy Plan the launch of our waitlist feature:
Phase 0 (Week 1): Internal testing — team registers for test events
Phase 1 (Week 2): Beta — 3 partner colleges, 100 students, collect feedback
Phase 2 (Week 3): Expanded beta — 10 colleges, monitor error rates
Phase 3 (Week 4): Full launch — all colleges, coordinated announcement

For each phase:
- Success criteria to advance to next phase
- Rollback triggers (error rate > 2%, negative feedback spike)
- Communication: what to tell users at each phase
- Monitoring: which metrics to watch hourly
```

### `@shipping-and-launch`
Checklist for actually shipping a feature.
```
@shipping-and-launch Run the shipping checklist for waitlist feature:
- [ ] PRD accepted by engineering lead
- [ ] All acceptance criteria tested and passed
- [ ] Load tested: 500 concurrent waitlist joins
- [ ] Feature flag: waitlist_feature off by default, enabled per college
- [ ] Rollback plan documented and tested
- [ ] Analytics events firing correctly (PostHog verified)
- [ ] Support team briefed on new feature
- [ ] Release notes written
- [ ] Announcement email scheduled
- [ ] Monitoring dashboard ready for launch day
```

---

## 💰 Monetization

### `@monetization` / `@pricing-strategy`
Revenue model design and pricing decisions.
```
@pricing-strategy Design pricing for our events platform:
Current: free for everyone

Options to evaluate:
A. Per-registration fee: ₹2-5 per ticket sold (% or flat)
B. Organizer subscription: ₹999/month for unlimited events
C. Freemium: free up to 50 registrations/event, paid for more
D. Institutional: ₹50,000/year per college institution
E. Hybrid: free for students, paid for organizers

For each: revenue projection at 500 colleges,
market fit, competitive comparison, student vs organizer impact.
Recommend the optimal pricing model for our growth stage.
```

### `@usage-based-pricing`
Metered pricing that scales with usage.
```
@usage-based-pricing Design a usage-based pricing model for event organizers:
Meter: number of registrations processed per month
Tiers:
- Free: 0-100 registrations/month (enough for small clubs)
- Starter ₹499/mo: 101-500 registrations
- Growth ₹1,499/mo: 501-2,000 registrations
- Scale ₹3,999/mo: 2,001-10,000 registrations
- Enterprise: custom

Include: how to meter, how to surface usage in dashboard,
how to handle month-end billing, overage policy.
```

### `@free-tier-strategy`
Designing a free tier that drives conversion.
```
@free-tier-strategy Design our free tier to maximize conversion to paid:
Free tier limits:
- Max 3 events published simultaneously
- Max 100 registrations per event
- Basic analytics only (registration count, no funnel)
- Email support only

Paid unlocks:
- Unlimited events and registrations
- Advanced analytics (funnel, cohorts, revenue)
- Custom registration form fields
- Priority support + dedicated account manager
- White-label option

Goal: free tier is genuinely useful for student clubs,
but growing events naturally hit limits and convert.
```

---

## 🔗 Complete Product Manager Prompt Chain

```
1️⃣  @brainstorming
    "Generate feature ideas for Q3: what would most improve student experience?"

2️⃣  @jobs-to-be-done-analyst
    "Validate top idea: is there a real unmet job to be done?"

3️⃣  @competitor-analysis
    "Does anyone solve this well already? Where can we differentiate?"

4️⃣  @to-prd
    "Write the full PRD for the chosen feature"

5️⃣  @before-you-build
    "Risk and assumption checklist before engineering starts"

6️⃣  @planning-and-task-breakdown
    "Break PRD into engineering tasks with story points"

7️⃣  @posthog-automation
    "Instrument analytics: define events, funnels, flags"

8️⃣  @launch-strategy
    "Phased rollout plan with success criteria per phase"

9️⃣  @kpi-dashboard-design
    "Design weekly PM dashboard for post-launch monitoring"

🔟  @pricing-strategy
    "Evaluate monetization model for the feature"
```

---

## 💡 Pro Tips for Product Managers

1. **`@to-prd` before any dev work** — an unreviewed PRD ships the wrong thing
2. **`@before-you-build` is a forcing function** — it surfaces assumptions you didn't know you had
3. **`@posthog-automation` at spec time** — instrument analytics before engineers write a line of code
4. **`@jobs-to-be-done-analyst` beats user stories** — it reveals the *why*, not just the *what*
5. **`@kpi-dashboard-design` defines success before you ship** — never launch without a measurement plan
