# How to Present The Prowler to Preston

**This is your playbook. Read it the night before. Read it the morning of. Don't read from it during the meeting — just know it.**

---

## The Meeting Structure

You have three acts. Each one builds on the last. Don't rush through them.

---

## Terminology Cheat Sheet — Study This First

Read through this so you're comfortable with every term. If Preston uses one of these, you should know it cold. If you use one, you should be able to explain it in one sentence.

### The Regulatory Landscape

| Term | What It Actually Means |
|---|---|
| **NIL** | "Name, Image, and Likeness." Since July 2021, college athletes can get paid for endorsement deals using their name, face, or personal brand. Before this, they couldn't make a dime. |
| **House v. NCAA** | A massive lawsuit (House v. NCAA) that settled in June 2025. It's the case that blew up the old system. The settlement says schools can now directly pay athletes from their revenue — up to about $20 million per school per year. This is brand new. |
| **College Sports Commission (CSC)** | A new independent organization created by the House settlement. Launched July 2025. They're the cops — they oversee and enforce NIL rules. They have the power to declare athletes ineligible. |
| **NIL Go** | The CSC's online portal where athletes are required to report their NIL deals. Think of it like an IRS filing system but for NIL deals. |
| **The $600 Rule** | Any NIL deal worth $600 or more must be reported to the CSC through NIL Go within 5 business days. Miss the deadline? Athlete could lose eligibility. This is the single biggest compliance headache in the industry right now. |
| **KYC** | "Know Your Customer." A legal requirement that says before you let someone do financial transactions on your platform, you have to verify they are who they say they are. Think: scanning a driver's license + taking a selfie to prove it's you. Banks do this. Venmo does this. We have to do this. |
| **1099-NEC** | An IRS tax form. If you pay someone more than $600 as an independent contractor (which is what athletes are classified as), you have to send them a 1099-NEC at the end of the year so they can file taxes. We automate this. |
| **W-9** | An IRS form that collects someone's taxpayer ID number (Social Security Number or EIN). Athletes fill this out before they can receive any payments. Required by law. |

### The Competitive Landscape

| Term | What It Actually Means |
|---|---|
| **Opendorse** | The biggest NIL marketplace platform right now. Owned by a company focused on athlete monetization. They help athletes find brand deals, manage contracts, and get paid. **What they DON'T do:** any kind of recruiting, scouting, or talent discovery for coaches. If a coach wants to find a player, Opendorse is useless. |
| **INFLCR** | Owned by Teamworks (a big sports tech company). It's a brand management app for athletic departments. Schools contract with INFLCR to help their existing athletes manage their brand. **What they DON'T do:** The athlete doesn't own their profile — the school does. And there's no marketplace for recruiting. |
| **MOGL** | An AI-powered platform that matches athletes with brands for campaigns. They recently launched an "AI agent" that automates campaign setup. **What they DON'T do:** No coach tools, no scouting, no stat-based search. A coach can't go on MOGL and say "find me a quarterback in Georgia." |
| **Stack Athlete** | Formerly called CaptainU. A recruiting profile platform — athletes build a profile page with their stats and film, then share it with coaches. **What they DON'T do:** No NIL deals, no marketplace, no AI, no compliance. It's basically a fancy website builder for athletes. |
| **SportsRecruits** | A recruiting communication platform — athletes can message coaches and vice versa. **What they DON'T do:** The tech is old. No video intelligence, no AI, no NIL anything. |

### The Technical Terms (for Act 2)

| Term | What It Means — Plain English |
|---|---|
| **API** | "Application Programming Interface." It's a set of rules that lets different software talk to each other. When the phone app needs to show an athlete's profile, it asks the API, and the API goes to the database and brings back the data. Think of the API as a waiter — the app (customer) tells the waiter what it wants, the waiter goes to the kitchen (database), and brings back the food (data). |
| **Backend** | The "behind the scenes" part of the app that users never see. It's the server, the database, the logic. When someone signs up, the backend saves their account. When a coach searches for an athlete, the backend runs the search. |
| **Frontend** | The part users actually see and touch. The mobile app is a frontend. The coach's web portal is a frontend. They're just pretty interfaces that talk to the backend. |
| **FastAPI** | The specific technology I'm using to build the backend. It's a Python framework known for being fast and modern. It's what powers the "brain" of the platform. |
| **Flutter** | The technology for the mobile app. It lets me write the app once and it runs on both iPhone and Android. Without Flutter, I'd have to build two separate apps — one for each platform — which would double the cost and time. |
| **Next.js** | The technology for the web portal (what coaches and brands use in their browser). It's built on React, which is the most popular web framework in the world. |
| **PostgreSQL** | The database. Where all the data lives — athlete profiles, stats, deals, contracts, everything. It's the most trusted open-source database in the industry. Free to use, rock solid, used by Instagram, Spotify, and the US government. |
| **pgvector** | An add-on for PostgreSQL that lets it do AI-style searches. Instead of searching by exact keywords, it can search by *meaning*. A coach types "fast defensive back from the south" and pgvector finds athletes that match that concept, even if those exact words aren't in the profile. |
| **Escrow** | Holding money in a neutral place until a condition is met. Brand pays $5,000 for a deal → we hold it → athlete does the post/appearance → we release the money. Nobody gets screwed. |
| **Stripe Connect** | Stripe is the biggest online payment processor (used by Amazon, Shopify, Lyft). Stripe Connect is their specific product for marketplaces — it handles collecting payment from one party, holding it, and paying it out to another. Perfect for our Brand → Platform → Athlete money flow. |
| **Persona** | A service that handles identity verification (KYC). When someone signs up for the marketplace, Persona checks their government ID and runs a selfie check to prove they're real. Prevents fraud. |
| **Twelve Labs** | An AI company that analyzes video. Their product (Pegasus) can watch game film and understand what's happening — touchdowns, interceptions, blocks — and auto-cut highlight clips. No human editor needed. |
| **RAG** | "Retrieval-Augmented Generation." A way to build an AI assistant that answers questions using your own data instead of making things up. The coach types a question → the system retrieves relevant athletes from the database → the AI generates a useful answer with ranked results. |
| **Embedding** | A way to turn text or video into a list of numbers that represent its *meaning*. Once something is an embedding, a computer can compare it to other embeddings and find similar things. This is how the AI search works under the hood. |
| **Moat** | Business term. It means a competitive advantage that's hard for others to copy. Like a moat around a castle. Our moat is the compliance automation — it's hard to build, it's legally complex, and athletes *need* it. |

---

## Act 1: The Problem (5 minutes)

Start here. Don't open with tech. Open with pain.

### What You Need to Understand

The college sports world just went through a complete revolution. Here's the story:

**Before July 2021:** College athletes couldn't make any money from their name. A football player with a million Instagram followers couldn't take a $500 deal from a local car dealer. The NCAA would strip their eligibility.

**July 2021 — NIL Rules Change:** The NCAA finally allowed athletes to profit from their Name, Image, and Likeness. Suddenly athletes could do endorsements, social media deals, appearances. But there were no rules, no infrastructure, no systems. The Wild West.

**June 2025 — House v. NCAA Settlement:** This is the big one. A class-action lawsuit forced the NCAA to completely restructure. Now schools can directly share revenue with athletes (up to ~$20M per school). The old "amateur" model is officially dead.

**July 2025 — College Sports Commission (CSC) Launches:** As part of the House settlement, a new independent body was created to actually enforce the rules. They launched the "NIL Go" portal where athletes must report deals. They've already started investigating schools for unreported deals.

**The $600 Problem:** Any NIL deal worth $600+ has to be reported to the CSC within 5 business days. Athletes are 18-22 year-olds juggling school, practice, and suddenly running a small business. They're forgetting to file. They're losing eligibility over paperwork.

**The Market Fragmentation:** A bunch of platforms popped up to help — but every single one only does one thing. There's no single platform where an athlete can build their presence, get discovered by coaches, land brand deals, AND stay compliant. They'd need accounts on 4-5 different services.

**That's the problem.** Fragmented tools, high-stakes compliance that athletes are failing at, and a $2.75 billion market growing fast with no one commanding it.

### What to Say

> "Preston, I spent the last week going deep on every platform in this space. I pulled apart Opendorse, MOGL, INFLCR, Stack Athlete, SportsRecruits — all of them.
>
> Here's what I found: every single one solves one piece of the puzzle. Opendorse handles NIL deals but has zero recruiting tools — a coach can't go on there and find talent. Stack Athlete does recruiting profiles but has no marketplace, no AI, no compliance. MOGL does AI brand-matching but a coach can't search by stats or watch film.
>
> Nobody has put all of it under one roof.
>
> And now the stakes are higher than ever. The House settlement dropped in June. The CSC launched in July. Any deal over $600 has to be reported to NIL Go within 5 business days or the athlete risks losing eligibility. These are 19-year-old kids trying to run a business while playing a sport and going to class. They're failing at the compliance part.
>
> That's the gap. The platform that combines recruiting, NIL marketplace, and automated compliance — that platform doesn't exist yet. We build it."

### Key Numbers — Know These Cold

| Number | What It Means |
|---|---|
| **$2.75 billion** | The size of the NIL market as of 2025. It roughly doubled from the year before. |
| **$20 million/year** | How much each school can now share directly with athletes under the House settlement. |
| **$1,500** | The average NIL deal size. These aren't all million-dollar contracts — most are local businesses paying athletes for social media posts and appearances. |
| **5 business days** | The deadline to report deals ≥$600 to the CSC via NIL Go. Miss it = potential loss of eligibility. |
| **$600** | The threshold that triggers mandatory reporting to the CSC AND triggers IRS tax reporting (1099-NEC). |

---

## Act 2: The Solution (10 minutes)

Now you open the STRATEGY.md on your screen. Scroll through the diagrams. Don't read the doc to him — use the visuals as anchors and talk through them.

### What You Need to Understand

The Prowler is three products in one:

**1. A Recruiting Platform (like LinkedIn for athletes)**
- Athletes build profiles with stats, bio, school info, and game film
- Coaches search for athletes by sport, position, location, graduation year, stats
- AI helps coaches find athletes by typing natural language questions
- This is the core — the thing that has to work first

**2. An NIL Marketplace (like Etsy for brand deals)**
- Brands browse athletes and create deal offers (e.g., "We'll pay you $2,000 to post about our product")
- Athletes can accept or decline
- Money is held in escrow until the athlete fulfills the deal (makes the post, does the appearance)
- Platform takes a 10% fee on every deal

**3. A Compliance Engine (the moat — nobody else has this)**
- Automatically detects when a deal is ≥$600
- Auto-generates the CSC disclosure paperwork
- Auto-submits to the NIL Go portal within the 5-day window
- Auto-generates 1099 tax forms at year-end
- Athlete never has to think about compliance — the platform handles it

The architecture is designed so one backend system ("the brain") powers both the mobile app (for athletes and parents) and the web portal (for coaches, scouts, and brands). One database, one set of rules, one source of truth. If an athlete updates their stats on their phone, the coach sees the update instantly on their laptop.

### How to Walk Him Through the Diagrams

**Scroll to the system architecture diagram and say:**

> "Here's the architecture. One backend brain built on FastAPI — that's a high-performance Python framework — serving both a mobile app and a web portal. One database holds everything. The mobile app is Flutter, so we build it once and it runs on both iPhone and Android — saves us 30-40% on development cost compared to building two separate apps.
>
> The coach portal is built with Next.js, which is a React framework optimized for fast loading and SEO — when an athlete's public profile shows up in Google, that matters."

**Scroll to the athlete journey diagram and say:**

> "This is what the athlete experiences. They sign up on their phone. Build their profile — stats, bio, school, position. Upload game film. Our AI — powered by a company called Twelve Labs — watches the film and automatically identifies the best plays and cuts them into highlight clips. No editing required.
>
> Now they're discoverable. Coaches can find them through search, or through the AI co-pilot, where they literally just type a question like 'find me a defensive back in Texas with a 4.4 forty.' The system searches by both hard stats and the semantic content of scouting reports and video tags."

**Scroll to the compliance flowchart and say:**

> "This is the part I want you focused on — this is the moat. When a brand creates a deal and an athlete accepts it, the platform automatically checks: is this deal $600 or more? If yes, it auto-generates the CSC disclosure, auto-submits it to NIL Go within the required 5-day window, and queues the 1099 tax form for year-end.
>
> The athlete doesn't fill out any paperwork. The brand doesn't worry about compliance. We handle it all programmatically.
>
> No one else does this. This is what makes The Prowler a necessity, not a nice-to-have."

**Scroll to the payment flow diagram and say:**

> "The money flow. Brand creates a deal for, say, $5,000. The money gets authorized through Stripe Connect — that's the payment processor used by Amazon, Shopify, Lyft — and held in escrow. The athlete does their deliverable. We verify it's done. Then we capture the payment, take our 10% platform fee, and the rest goes straight to the athlete's bank account.
>
> On a $1,000 deal: Stripe takes $29.30 for processing and $2.75 for the payout. We take $100 as our platform fee. The athlete gets $867.95. Clean, transparent, automated."

### Key Phrases to Use — Preston Will Connect With These

| Phrase | Why It Lands |
|---|---|
| "The moat is compliance" | Preston's CMMC background means he understands that regulatory complexity = competitive advantage. |
| "Athletes are losing eligibility over paperwork" | This is the pain point. Real kids, real consequences. |
| "We automate the entire chain" | Shows this isn't a manual process — the platform handles it end-to-end. |
| "One brain, multiple faces" | Simple metaphor for the architecture. One backend, multiple frontends. |
| "Nobody has put all of it under one roof" | The positioning statement. Memorize it. |
| "This isn't a nice-to-have, it's insurance" | Reframes the product from a tool to a necessity. |

---

## Act 3: The Business (10 minutes)

This is where you show the weight of the work AND your proposed deal.

**What to say:**

> "Let me be honest with you about what this takes to build. I itemized every feature, every hour. It's 2,020 hours of development across four phases. At market rate — $125 an hour, which is the median for this skill set — that's $252,500.
>
> Phase 1 alone is 524 hours and $65,500. That's the foundation — profiles, search, auth, mobile app, coach portal, everything deployed.
>
> Infrastructure is cheap. Phase 1 runs at $160 a month. The expensive stuff — video AI, payment processing — doesn't kick in until Phase 2 and 3.
>
> Now — I know you're not writing a $252K check. Neither would I. So here's what I want to propose."

---

## The Deal Pitch

This is the most important part. You're not discounting your work. You're restructuring how you get paid.

**What to say:**

> "I believe in this project enough to put my own money on the line by investing my time. Here's what I'm proposing..."

### Option A: The "Technical Co-Founder" Model (Recommended)

> "Instead of $125 an hour, I'll work at $50 an hour — cost of living, keeps the lights on. That drops Phase 1 from $65,500 down to $26,200. The full build goes from $252K down to about $101K.
>
> In exchange, I take a 30% equity stake in The Prowler. I'm not an employee. I'm a partner. I build it, I maintain it, I scale it. If this works and we're doing $80K a month in revenue, that equity is worth a hell of a lot more than the hourly rate I'm discounting.
>
> I also want a 5% revenue share on all platform transaction fees in perpetuity. So when the marketplace goes live and we're taking 10% on every NIL deal, I get 5% of that 10% — half a point on every deal, forever. That's my passive income."

Here's how the math works to show him:

| | Full Rate | Discounted Rate | Difference |
|---|---|---|---|
| **Hourly rate** | $125/hr | $50/hr | -$75/hr |
| **Phase 1** | $65,500 | $26,200 | You save $39,300 |
| **Phase 2** | $64,500 | $25,800 | You save $38,700 |
| **Phase 3** | $82,500 | $33,000 | You save $49,500 |
| **Phase 4** | $40,000 | $16,000 | You save $24,000 |
| **Total** | **$252,500** | **$101,000** | **Preston saves $151,500** |

What you get back:
| Your Compensation | Details |
|---|---|
| **Cash (reduced rate)** | $101,000 over ~20 months (~$5,050/mo) |
| **30% equity** | Ownership stake. Grows with the company. |
| **5% of platform revenue** | Passive income. Every NIL deal, every subscription. Forever. |

**Revenue share math at scale:**

| Monthly Platform Revenue | Your 5% Share |
|---|---|
| $10,000/mo (early Phase 3) | $500/mo |
| $50,000/mo (growing) | $2,500/mo |
| $80,000/mo (target Year 2) | $4,000/mo |
| $200,000/mo (scale) | $10,000/mo |

---

### Option B: The "Deferred Comp" Model (if he pushes back on equity %)

> "If 30% equity feels like too much, here's another structure. I work at $75 an hour — that's 40% below market. Total build drops to $151,500. The $101K difference between that and market rate gets logged as deferred compensation. When revenue hits, I get paid back from profits before any distributions. Plus I still want 15-20% equity and a 3% revenue share."

| | Full Rate | Discounted Rate |
|---|---|---|
| **Hourly rate** | $125/hr | $75/hr |
| **Total** | $252,500 | $151,500 |
| **Deferred comp** | — | $101,000 (paid from profits) |
| **Equity** | — | 15-20% |
| **Revenue share** | — | 3% |

---

### Option C: The "Phase-by-Phase" Model (if he needs to start small)

> "Let's not commit to the whole thing. Pay me for Phase 1 at full rate — $65,500. We prove the core product works. If it does, we negotiate the long-term deal for Phases 2-4 with equity and revenue sharing. No risk for either of us."

---

## What to Push Back On

Preston will negotiate. Here's what to expect and how to handle it:

**"Can you do it cheaper?"**
> "I already cut my rate by 60%. The $50/hr rate is below what junior developers charge. The equity and revenue share is how I make that work — I'm betting on the product alongside you."

**"30% equity is too much for a developer."**
> "I'm not just a developer. I'm designing the architecture, building the infrastructure, writing every line of code, handling deployment, and maintaining a platform that processes money and legal contracts. If you hired a CTO and a dev team to do this, you'd be paying $300K-$500K a year in salaries. I'm one person doing all of it for a share of the upside."

**"What if it doesn't work?"**
> "Then I've worked thousands of hours at $50 an hour and you've paid under market for a fully built platform. I'm the one taking the bigger risk. My equity is worth zero if this doesn't generate revenue."

**"Can't you just do it for equity only?"**
> "No. I have bills. The reduced rate keeps me focused on this full-time without taking other projects. If I'm doing this at zero cash, I'm splitting my time with paying clients and this project takes three years instead of twenty months."

**"I want to own all the IP."**
> "IP stays with The Prowler, LLC — the company we both own. My equity represents my ownership stake in that IP. If I leave, the code stays. Standard technical co-founder terms."

---

## How to Close

> "Preston, here's the bottom line. You've got the vision, the sports background, the regulatory knowledge. I've got the engineering. Together we fill every gap.
>
> The full build at market rate is a quarter million dollars. I'm offering to do it for $101K cash plus equity and a revenue share, because I think this is real and I want to be part of it long-term.
>
> Phase 1 is $26,200 and five months. That gets us a working product with athletes on it and coaches using it. We prove it works, we keep building. What do you think?"

---

## Before the Meeting Checklist

- [ ] Send Preston the GitHub link to STRATEGY.md a day before so he can read it
- [ ] Have the doc open on your screen during the meeting
- [ ] Know the three big numbers cold: $252K full rate, $101K discounted, $26,200 for Phase 1
- [ ] Know the competitor names and what each one misses
- [ ] Have a clean one-page deal term sheet ready (see below)

---

## One-Page Term Sheet to Hand Him

Print this or send it after the meeting:

---

### The Prowler — Development Partnership Terms

**Parties:** Preston Pritchard (Founder) & Ben Hutton / Hutton Technologies (Technical Partner)

| Term | Details |
|---|---|
| **Scope** | Full platform development — Phases 1 through 4 as defined in STRATEGY.md |
| **Development Rate** | $50/hr (market rate: $125/hr) |
| **Estimated Total Cash** | ~$101,000 over ~20 months |
| **Phase 1 Cash Commitment** | $26,200 (524 hours) |
| **Equity Grant** | 30% ownership stake in The Prowler LLC |
| **Revenue Share** | 5% of gross platform revenue, paid monthly, in perpetuity |
| **Vesting** | Equity vests over 24 months with a 6-month cliff |
| **IP Ownership** | All code and IP owned by The Prowler LLC |
| **Departure Terms** | Vested equity retained; unvested forfeited. Code stays with the company. |
| **Maintenance** | Ongoing platform maintenance included for 12 months post-Phase 4 completion |

*Both parties should retain independent counsel before signing any agreement.*

---

*This is your deal to make. Go get it.*
