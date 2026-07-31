# 📈 SEO & Growth Marketer — Skills Guide

SEO and growth marketers drive organic traffic, optimize conversion, and build growth engines that compound over time. This guide covers technical SEO, content strategy, programmatic SEO, growth loops, social media, and email marketing.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| Technical SEO | `@seo-technical`, `@seo-audit`, `@frontend-seo`, `@seo-sitemap`, `@schema-markup-generator` |
| Content SEO | `@seo-content-writer`, `@seo-keyword-strategist`, `@seo-aeo-blog-writer` |
| Programmatic SEO | `@programmatic-seo`, `@seo-programmatic`, `@landing-page-generator` |
| Growth | `@growth-engine`, `@lead-magnets`, `@referral-program`, `@viral-generator-builder` |
| CRO | `@ab-testing`, `@ab-test-setup`, `@cro` |
| Social Media | `@linkedin-post-writer`, `@instagram`, `@twitter-automation`, `@youtube-seo-optimizer` |
| Email Marketing | `@email-sequence`, `@email-systems`, `@mailchimp-automation`, `@brevo-automation` |
| Analytics | `@google-analytics-automation`, `@posthog-automation`, `@mixpanel-automation` |
| Link Building | `@seo-authority-builder` |
| Local / International | `@seo-geo`, `@seo-hreflang`, `@local-legal-seo-audit` |

---

## 🔍 Technical SEO

### `@seo-technical`
Comprehensive technical SEO implementation.
```
@seo-technical Perform a technical SEO audit of our events platform (events.college.edu):

Crawlability:
- robots.txt: are we blocking any important pages?
- sitemap.xml: is it generated, submitted to Search Console, and accurate?
- Internal linking: are event pages linked from category pages?

Indexability:
- Meta robots: no-index accidentally on event pages?
- Canonical tags: are duplicate URL variants (with/without www, trailing slash) handled?
- JavaScript rendering: do event pages require JS to render content?
  (Googlebot can render JS but delays it — server-side render event data)

Performance (Core Web Vitals):
- LCP target: < 2.5s on mobile (event hero image is likely culprit)
- INP target: < 200ms (registration button click)
- CLS target: < 0.1 (images with no size attributes?)

Structured data:
- Event schema: all event pages have JSON-LD Event markup?
- BreadcrumbList: navigation hierarchy marked up?

HTTPS:
- All pages HTTPS, no mixed content
- HSTS header present

Mobile:
- Viewport meta tag correct?
- Font sizes readable without pinch-zoom?
- Tap targets ≥ 48px?

Output: prioritized fix list with implementation code.
```

### `@seo-audit`
Holistic SEO audit covering technical + content + authority.
```
@seo-audit Run a comprehensive SEO audit for our events platform:

Technical: [use @seo-technical for detail]
Content: are event pages unique and substantial? Or thin content?
Authority:
- How many external sites link to us?
- What is our domain authority? (use Moz or Ahrefs benchmark)
- Competitor backlink gap analysis

Priority matrix:
- Quick wins (high impact, low effort): fix in sprint 1
- Medium wins (medium impact, medium effort): plan for Q2
- Long-term investments (high impact, high effort): SEO roadmap
```

### `@frontend-seo`
SEO implementation in the frontend codebase.
```
@frontend-seo Implement SEO best practices in our Next.js app:

For each event page (/events/[slug]):
- Dynamic <title>: "[Event Name] | College Events"
- Dynamic <meta description>: 150-160 chars with event name, date, venue
- Open Graph: og:title, og:description, og:image (event banner), og:type: "event"
- Twitter Card: summary_large_image
- Canonical: self-referencing canonical URL
- Structured Data: Event JSON-LD (see @schema-markup-generator)

For category pages (/events/category/[category]):
- Title: "[Category] Events at [College Name]"
- Meta description: dynamic based on upcoming event count
- Pagination: rel="next" / rel="prev" links

Global:
- robots.txt: allow all, exclude /admin, /api
- sitemap.xml: auto-generated, includes all published events + category pages
- Generate sitemap on build + webhook trigger when event published

Show: Next.js implementation with generateMetadata() and JSON-LD component.
```

### `@schema-markup-generator`
Generate structured data markup for Google rich results.
```
@schema-markup-generator Add Event schema markup to all event pages:

JSON-LD to generate for each event:
{
  "@context": "https://schema.org",
  "@type": "Event",
  "name": "[Event Title]",
  "description": "[Event Description — 150-500 chars]",
  "startDate": "[ISO 8601 with timezone: 2025-12-15T18:00:00+05:30]",
  "endDate": "[ISO 8601]",
  "eventAttendanceMode": "https://schema.org/OfflineEventAttendanceMode",
  "eventStatus": "https://schema.org/EventScheduled",
  "location": {
    "@type": "Place",
    "name": "[Venue Name]",
    "address": { "@type": "PostalAddress", "addressLocality": "[City]" }
  },
  "organizer": { "@type": "Organization", "name": "[Organizer Name]" },
  "image": "[Event Banner URL — 1200x630px recommended]",
  "url": "[Canonical event URL]",
  "offers": {
    "@type": "Offer",
    "price": "[price or 0 for free]",
    "priceCurrency": "INR",
    "availability": "https://schema.org/InStock",
    "url": "[Registration URL]",
    "validFrom": "[Registration open date ISO 8601]"
  }
}

Show: Next.js component that generates this dynamically from event data.
Test with: Google Rich Results Test.
```

### `@seo-sitemap`
Generate and maintain XML sitemaps.
```
@seo-sitemap Generate an XML sitemap for our events platform:

Sitemap strategy (split into 3):
1. sitemap-pages.xml: static pages (home, about, contact, /events category pages)
2. sitemap-events.xml: all published, future events (dynamic, rebuilt on publish)
3. sitemap-index.xml: points to the two above

For events sitemap:
- Include: <loc>, <lastmod> (event updatedAt), <changefreq>weekly, <priority>0.8
- Exclude: draft, cancelled, past events (> 30 days ago)
- Max: 50,000 URLs per sitemap file

Next.js implementation:
- app/sitemap.ts: generate sitemap-pages
- app/sitemap-events.ts: fetch from DB, generate events sitemap
- Auto-regenerate when event is published (webhook to revalidate)
- Submit to: Google Search Console, Bing Webmaster Tools
```

---

## 📝 Content SEO

### `@seo-keyword-strategist`
Research and organize target keywords.
```
@seo-keyword-strategist Research keywords for our college events platform:
Target: college students in India searching for campus events

Seed keywords: "college events", "campus events app", "student events near me"

Keyword research deliverables:
1. Head terms (Volume > 10K/month):
   - "college events" — competition high, target with homepage
   - "events near me" — broad, but we can target with location
   
2. Mid-tail (Volume 1K-10K):
   - "[city] college events" — city-specific pages
   - "free college events" — category filter page
   - "college cultural fest" — event type pages
   
3. Long-tail (Volume < 1K, low competition):
   - "how to register for college events online"
   - "college event management app India"
   - "best college events app for students"
   
4. Question queries (good for AEO/featured snippets):
   - "how to find events happening at my college"
   - "what is the best app for college events"

For each: volume, competition score, intent (informational/navigational/transactional), target page.
```

### `@seo-content-writer`
Write SEO-optimized content that ranks and converts.
```
@seo-content-writer Write an SEO-optimized page for: "Engineering Events in Bangalore"

Target keyword: "engineering events bangalore"
Secondary: "engineering college events", "tech events for students bangalore"

Page requirements:
- Title tag: "Engineering Events in Bangalore 2025 | Find & Register | College Events"
- H1: "Engineering Events in Bangalore"
- Introduction: 100 words, includes primary keyword, hooks the reader
- Body: 600-800 words covering:
  - Types of engineering events (hackathons, workshops, fests)
  - Upcoming engineering events list (dynamic)
  - How to register (with CTA)
  - Why attend (career, networking, skills)
- Semantic keywords to include: hackathon, IEEE, coding competition, robotics, tech workshop
- Internal links: to specific engineering event pages, to hackathon category
- CTA: "Browse All Engineering Events →"

Not keyword-stuffed. Genuinely helpful to a student searching this.
```

### `@seo-aeo-blog-writer`
Write for AI answer engines (ChatGPT, Gemini, Perplexity citations).
```
@seo-aeo-blog-writer Write a blog post optimized for AEO (Answer Engine Optimization):
Topic: "How to Organize a College Event: Complete Guide for Student Organizers"
Target: appear in ChatGPT/Perplexity answers when students ask about event planning

Structure for AEO:
- Answer the question directly in first 2 sentences (featured snippet bait)
- Use numbered lists and clear headers (easily parseable by AI)
- Include factual claims that can be cited
- FAQ section at the end with common questions
- 1,500+ words (comprehensive = authoritative)

Keywords: "how to organize college event", "college event planning checklist",
"student event management tips"
```

---

## ⚡ Programmatic SEO

### `@programmatic-seo`
Create hundreds of targeted landing pages at scale.
```
@programmatic-seo Design a programmatic SEO strategy for our events platform:

Page templates to create at scale:
1. City × Category pages: 50 cities × 10 categories = 500 pages
   Template: "Music Events in [City]", "Sports Events in [City]"
   Data: auto-populated from our events database
   
2. College-specific pages: one page per college
   Template: "Events at [College Name] — [City]"
   Auto-generated from events organized at each college
   
3. Event type pages: "Hackathons", "Cultural Fests", "Academic Workshops"
   Long-form category pages with upcoming events + guide content

Implementation:
- Next.js generateStaticParams for city/category combinations
- Only generate page if ≥ 3 upcoming events (no thin content)
- Regenerate on schedule (weekly) + on new event publish
- Unique meta title and description per page (templated but distinct)
- Canonical: self-referencing
- noindex if 0 events currently (dynamic)

Scale: 500+ indexable pages from database-driven templates.
```

### `@landing-page-generator`
Generate high-converting landing pages for campaigns.
```
@landing-page-generator Generate landing pages for our "Back to College" campaign:
Target: students returning to campus in August

Page variants:
1. /back-to-college — general student
2. /back-to-college/engineering — engineering students
3. /back-to-college/bangalore — Bangalore colleges

Each page:
- Headline: "Your College Life Starts Here"
- Subheadline: "Discover events, register in seconds, never miss out"
- Hero: 3 images of students at events
- Social proof: "10,000 students have already discovered events this semester"
- Features: search, instant registration, QR tickets
- CTA: "Browse Events Now" → /events
- UTM parameters for tracking campaign performance
```

---

## 🚀 Growth Engine

### `@growth-engine`
Design a compounding growth strategy.
```
@growth-engine Design a growth strategy for our events platform:
Current: 500 students, 1 college
Goal: 5,000 students, 10 colleges in 6 months

Map all growth levers:

Acquisition (how new students find us):
1. SEO: "[college name] events" pages — long-term organic
2. Social: event organizers share on Instagram/WhatsApp (viral potential)
3. Partnership: college student unions officially adopt our platform
4. Content: "What's Happening This Week" email/social series

Activation (first value moment):
- Goal: student discovers and registers for first event within 24 hours
- Onboarding: show 3 personalized events immediately on signup

Retention (keep coming back):
- Weekly digest email: "5 events this week matching your interests"
- Push notification: "Registration opens in 1 hour for [event you viewed]"

Referral (students invite others):
- Built-in: "Share my ticket" → recipient sees event, signs up
- Incentive: first event free with referral code

Revenue (paid organizer subscriptions):
- Convert when organizer hits free tier limits

Prioritize: which levers give us the first 1,000 students fastest?
```

### `@referral-program`
Design and implement a referral growth loop.
```
@referral-program Design a referral program for our events platform:
Mechanic: student invites a friend → friend signs up + registers for 1 event → both get reward

Reward options (evaluate each):
A. Discount: ₹50 off next paid event (both parties)
B. Priority access: invited friends get early registration for popular events
C. Social: "Invited by [Name]" badge on their profile

Implementation:
- Unique referral link per user: events.college.edu/join?ref=ABC123
- Referral landing page: shows who invited them + social proof
- Attribution: cookie + server-side tracking
- Fraud prevention: email verification, device fingerprinting
- Dashboard: user sees how many friends joined via their link

Metric: K-factor (viral coefficient). Target: K > 0.3 (each user brings 0.3 more users)
```

### `@viral-generator-builder`
Build viral content and sharing mechanics.
```
@viral-generator-builder Create viral sharing mechanics for our events platform:

Mechanic 1: "I'm Going" social card
- After registration, show shareable image card
- Card: event name, date, student's name, "I'm going 🎉"
- Customized with event colors/image
- One-click share to Instagram Story, WhatsApp, Twitter
- UTM tracking: track signups from shares

Mechanic 2: Countdown timer embed
- Organizers get embeddable countdown: "X days until [event]"
- Shareable on college WhatsApp groups
- Click → event registration page

Mechanic 3: QR ticket flex
- Beautiful, shareable ticket image (not just functional QR)
- "Just got my ticket to [event]!" — designed to be shared
- Link in shared image → 20% of viewers register
```

---

## 📱 Social Media

### `@linkedin-post-writer`
LinkedIn content for B2B reach (colleges, student unions).
```
@linkedin-post-writer Write 3 LinkedIn posts for different stages of our growth:

Post 1: Launch announcement (for college administrators)
- Hook: a problem all college administrators face
- Story: how we saw students missing out on events because of outdated bulletin boards
- Solution: 3 sentences on what we built
- CTA: "Book a 15-minute demo"
- 200-250 words, no hashtag spam (3 max)

Post 2: Social proof (after 10 colleges)
- Milestone: "10 colleges. 10,000 students. ₹5 Lakh in events registered."
- Key metric spotlight
- Quote from one organizer
- CTA: "Is your college next?"

Post 3: Thought leadership
- Contrarian take: "Student events are dying at most colleges"
- Why: outdated discovery, manual registration, no data
- Our perspective: what modern student engagement looks like
- Soft CTA: link to blog post
```

### `@instagram`
Instagram content strategy and captions.
```
@instagram Create an Instagram content calendar for our events platform launch:
Account: @collegeeventsapp (targeting students, 18-22)

Content pillars:
1. Event Spotlight (40%): highlight upcoming events, behind-the-scenes
2. Student Stories (30%): testimonials, "Day in the life" attending events
3. Tips & Info (20%): "How to find events", "5 events this weekend"
4. Product Features (10%): feature showcase Reels

Week 1 content:
- Monday: Reel — "3 events happening this week" (quick cuts, trending audio)
- Wednesday: Carousel — "Meet the organizer behind [popular event]"
- Friday: Story poll — "Attending [event] this weekend? 🙋"
- Sunday: Quote graphic — motivational about college experiences

Hashtag strategy: 5 niche (#collegehackathon) + 5 medium (#campuslife) + 2 broad (#college)
Caption voice: casual, friendly, uses 1-2 emojis, ends with question for engagement
```

---

## 📧 Email Marketing

### `@email-sequence`
Build automated email sequences.
```
@email-sequence Build our student onboarding email sequence:
Trigger: student signs up for the first time

Email 1 (immediate): Welcome + personalization ask
Subject: "Welcome to College Events! Tell us what you love 🎉"
- Welcome, brief value prop
- 3 interest category buttons (in-email click → updates preferences)
- Preview: "Based on your interests, we found these for you..."

Email 2 (Day 2, if no registration): Gentle nudge with personalized events
Subject: "3 [Interest] events happening this week"
- Show 3 events matching their selected interests
- Each with: image, title, date, spots remaining, "Register" CTA

Email 3 (Day 7, if still no registration): Social proof + FOMO
Subject: "500 students registered for events this week"
- Social proof + what they're missing
- Surface a "trending" event (most viewed this week)
- "Your classmates are going to [popular event] →"

Email 4 (Day 14, if registered for 1st event): Engagement deepening
Subject: "You're going to [Event Name]! Here's what to know"
- Event reminder: date, time, venue (with map link)
- QR ticket download
- "More events you might like"

Goal for sequence: 40% of new users register within 14 days.
```

### `@brevo-automation` / `@mailchimp-automation`
Configure email platform automations.
```
@brevo-automation Set up Brevo (formerly Sendinblue) for our events platform:

Lists / Contacts:
- Segment: students by interest (music, sports, academic, cultural)
- Segment: organizers (separate product track)
- Segment: by college (for college-specific announcements)

Automations:
1. Welcome sequence: [as designed above]
2. Event reminder: 24h before registered event → send reminder email
3. Re-engagement: no login in 30 days → "What's new" email
4. Win-back: no registration in 60 days → "3 free events near you" + incentive

Transactional emails (via API, not marketing):
- Registration confirmation: immediate, includes QR ticket
- Waitlist notification: when spot opens
- Organizer: new registration received

Analytics to track: open rate, click rate, conversion to registration, unsubscribe rate.
```

---

## 🔗 Complete Growth Marketer Prompt Chain

```
1️⃣  @seo-technical
    "Fix crawlability, Core Web Vitals, canonical, robots.txt"

2️⃣  @schema-markup-generator
    "Add Event JSON-LD to all event pages for Google rich results"

3️⃣  @seo-sitemap
    "Generate and submit sitemap: pages + events"

4️⃣  @seo-keyword-strategist
    "Research keyword clusters: city×category, college-specific"

5️⃣  @programmatic-seo
    "Build 500+ targeted landing pages from database templates"

6️⃣  @seo-content-writer
    "Write SEO content for top 10 category and city pages"

7️⃣  @growth-engine
    "Design viral/referral loops and multi-channel acquisition"

8️⃣  @referral-program
    "Implement student referral program with tracking"

9️⃣  @email-sequence → @brevo-automation
    "Build onboarding and re-engagement email sequences"

🔟  @ab-testing → @posthog-automation
    "A/B test landing page headlines, track conversion funnel"
```

---

## 💡 Pro Tips for SEO & Growth Marketers

1. **`@schema-markup-generator` for events is highest-ROI SEO work** — Event rich results get 20-30% higher CTR
2. **`@programmatic-seo` scales your content 100x with one template** — 500 pages from a database query
3. **`@growth-engine` before any individual tactic** — understand your full acquisition map first
4. **`@email-sequence` is your highest-converting channel** — email beats social media for student activation
5. **`@seo-aeo-blog-writer`** — optimize for ChatGPT/Gemini citations now, not just Google
6. **`@referral-program` K-factor > 0.5 is a game changer** — makes every user acquire half a new user
