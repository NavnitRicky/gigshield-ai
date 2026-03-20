# GigShield — AI-Powered Parametric Insurance for India's Food Delivery Partners

> **Guidewire DEVTrails 2026 — Phase 1 Submission**  
> Team: GigShield | Platform: Web Application  
> **Status: Production-Ready Prototype with Live Weather & Anti-Fraud Security**

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Chosen Persona & Rationale](#chosen-persona--rationale)
3. [Problem Deep-Dive](#problem-deep-dive)
4. [Solution Overview](#solution-overview)
5. [Security & Anti-Fraud Architecture](#security--anti-fraud-architecture)
6. [Persona-Based Scenarios & Workflow](#persona-based-scenarios--workflow)
7. [Weekly Premium Model](#weekly-premium-model)
8. [Parametric Triggers](#parametric-triggers)
9. [AI/ML Integration Plan](#aiml-integration-plan)
10. [Fraud Detection Architecture](#fraud-detection-architecture)
11. [Tech Stack & Architecture](#tech-stack--architecture)
12. [Development Plan (6-Week Roadmap)](#development-plan-6-week-roadmap)
13. [Platform Choice Justification](#platform-choice-justification)
14. [Prototype Status](#prototype-status)
15. [How to Run Locally](#how-to-run-locally)
16. [API Reference](#api-reference)
17. [Data Sources & References](#data-sources--references)

---

## Executive Summary

**GigShield** is an AI-enabled parametric insurance platform that protects India's food delivery partners (Zomato, Swiggy, Zepto) against **income loss caused by external disruptions** — extreme weather, severe pollution, floods, unplanned curfews, and local strikes.

Unlike traditional insurance that requires claim filing and manual verification, GigShield uses **parametric triggers** — when weather data crosses predefined thresholds (e.g., temperature > 42°C, rainfall > 65mm/hr), claims are **automatically initiated and payouts are processed instantly** via UPI.

### Key Differentiators

- **Zero-touch claims**: No paperwork, no waiting — automated from trigger to payout
- **Weekly pricing**: ₹29–79/week aligned with gig worker payout cycles
- **AI risk scoring**: Dynamic premiums based on city, zone, vehicle type, working hours, and historical disruption patterns
- **Intelligent fraud detection**: Multi-factor anomaly scoring using GPS validation, claim frequency analysis, and weather data cross-referencing
- **Hyper-local coverage**: Zone-level parametric triggers, not just city-wide
- **Live weather integration**: Real-time data from Open-Meteo API across 7 Indian cities
- **Production-grade security**: Admin authentication, rate limiting, audit logging, weather cross-verification

---

## Security & Anti-Fraud Architecture

> **Critical for real-world deployment** — GigShield implements multi-layer security to prevent exploitation and money theft.

### Admin Authentication
- Session-based authentication with SHA-256 hashed passwords
- All write operations (create workers, policies, claims, approve payouts) require admin login
- Read-only dashboard and weather data accessible without auth
- 8-hour session expiry with automatic logout

### Anti-Fraud Protections

| Protection | How It Works |
|---|---|
| **Weather Cross-Verification** | Manual claims are verified against live weather data — if no matching weather event is occurring in the worker's city, the claim is rejected |
| **24-Hour Claim Cooldown** | Workers cannot submit another claim within 24 hours of their last claim |
| **Weekly Claim Cap** | Maximum 2 claims per worker per week — prevents unlimited exploitation |
| **Earnings Cap** | Worker earnings capped at ₹15,000/week — prevents inflated payouts |
| **Income Loss Cap** | Maximum 12 hours of income loss per claim |
| **Payout Ceiling** | No payout can exceed the policy's max weekly coverage |
| **Policy Age Check** | Claims within 48h of policy creation get +20 fraud score |
| **High Fraud Payout Block** | Claims with fraud score ≥70 are blocked from payout even if manually approved |
| **Simulate Block (Production)** | `/api/simulate-trigger` is disabled in production mode |
| **Duplicate Policy Prevention** | Workers cannot have multiple active policies of the same tier |

### Rate Limiting
- 30 API requests per minute per IP address
- Prevents brute-force attacks and spam

### Audit Logging
- Every admin action is logged with timestamp, IP, actor, and result
- Accessible via `/api/audit-log` (admin only)
- Last 1000 entries retained

## Chosen Persona & Rationale

### Persona: Food Delivery Partners (Zomato / Swiggy / Zepto)

**Why food delivery?**

1. **Largest gig segment**: Zomato and Swiggy alone engage 15+ lakh delivery partners monthly ([Moneycontrol, Jan 2026](https://www.moneycontrol.com/news/business/startup/how-much-do-delivery-partners-actually-earn-a-look-inside-the-pay-model-for-gig-workers-on-zomato-swiggy-13756512.html))
2. **Maximum weather exposure**: Food delivery has the highest sensitivity to weather — riders spend 8-11 hours outdoors daily, directly exposed to heat, rain, and pollution
3. **Predictable earnings structure**: Base pay (₹20-50/order) + distance bonus (₹8-10/km beyond 5km) + incentives makes income loss quantifiable
4. **Tight weekly cycles**: Platforms pay weekly, making weekly premium billing natural
5. **Growing but unprotected**: India's gig workforce is projected to reach 23.5 million by 2030 ([NITI Aayog](https://www.niti.gov.in)), yet has zero income protection against climate disruptions

### Delivery Partner Profile

| Attribute | Typical Value |
|---|---|
| Daily earnings (gross) | ₹800 - ₹1,500 |
| Weekly earnings (full-time) | ₹4,000 - ₹6,000 |
| Monthly earnings (net, after fuel) | ₹15,000 - ₹21,000 |
| Daily working hours | 8-11 hours |
| Earnings per hour | ~₹102 (Zomato avg, 2025) |
| Vehicle type | Bike (70%), EV (15%), Bicycle (15%) |
| Income loss during heatwaves | Up to 40% |
| Income loss per °C rise | ~19% |

*Sources: [Moneycontrol](https://www.moneycontrol.com/news/business/startup/how-much-do-delivery-partners-actually-earn-a-look-inside-the-pay-model-for-gig-workers-on-zomato-swiggy-13756512.html), [EfD Initiative](https://www.efdinitiative.org/about-efd/efd-reports/efd-annual-report-2024/impact-stories/rising-temperatures-cause-lost-incomes), [Down To Earth](https://www.downtoearth.org.in/climate-change/riders-in-the-heat-indias-gig-workforce-at-boiling-point)*

---

## Problem Deep-Dive

### The Income Loss Crisis

India's food delivery partners face a cruel paradox: the days they need income most (extreme weather = people ordering in) are often the days they physically cannot work.

**Research findings:**
- Workers experience a **19% decrease in net earnings for every 1°C increase** in wet-bulb temperature ([EfD Initiative](https://www.efdinitiative.org/about-efd/efd-reports/efd-annual-report-2024/impact-stories/rising-temperatures-cause-lost-incomes))
- During heatwaves, earnings **drop by up to 40%** ([EfD study of 400 workers in Delhi, 2019](https://www.efdinitiative.org/about-efd/efd-reports/efd-annual-report-2024/impact-stories/rising-temperatures-cause-lost-incomes))
- Delhi summer 2024 recorded temperatures exceeding **49°C** — delivery platforms saw significant rider shortages
- Gig workers in Hyderabad spend **6-8 hours outdoors in 45-50°C conditions** without access to shade or healthcare ([Down To Earth](https://www.downtoearth.org.in/climate-change/riders-in-the-heat-indias-gig-workforce-at-boiling-point))
- Heat-related **medical expenses increase 14% per degree of temperature rise**, reaching **25% on heatwave days**

### Why Current Solutions Fail

| Current State | Problem |
|---|---|
| No insurance exists | Gig workers bear 100% of income loss risk |
| Traditional insurance | Requires claim filing, documentation, weeks of processing |
| Platform pause compensation | Minimal or non-existent — "If I don't log in, I don't earn" |
| Government schemes | Not designed for app-based gig economy |

**GigShield fills this gap with automated, parametric coverage specifically designed for the gig earnings cycle.**

---

## Solution Overview

### How GigShield Works

```
[Disruption Occurs] → [Parametric Trigger Detected] → [AI Validates] → [Auto-Claim Created] → [Fraud Check] → [Instant Payout via UPI]
     (Weather API)      (Threshold exceeded)          (Risk score)     (Zero-touch)           (Score < 30)     (< 60 seconds)
```

### Core Flow

1. **Onboarding**: Worker registers with platform, city, zone, vehicle type, and earnings data
2. **Risk Profiling**: AI calculates a risk score (0-100) based on location risk, vehicle type, working hours, and experience
3. **Policy Selection**: Choose Basic (₹29/wk), Standard (₹49/wk), or Premium (₹79/wk) coverage
4. **Dynamic Premium**: AI adjusts the exact premium using the worker's personal risk score
5. **Monitoring**: Real-time weather/disruption data is continuously checked against parametric thresholds
6. **Auto-Claim**: When thresholds are breached, claims are automatically generated for affected workers
7. **Fraud Detection**: AI scores each claim for anomalies (0-100). Low-risk claims are auto-approved
8. **Instant Payout**: Approved claims trigger immediate UPI transfer of lost income compensation

---

## Persona-Based Scenarios & Workflow

### Scenario 1: Heatwave in Delhi (Rajesh, Zomato Rider)

**Profile**: Rajesh Kumar, bike rider, South Delhi zone, 10h/day, ₹4,500/week  
**Event**: Delhi hits 47°C (threshold: 42°C) — IMD issues extreme heat warning  

**Flow**:
1. OpenWeatherMap API reports 47°C for South Delhi → **Parametric trigger fires**
2. GigShield creates a weather alert: "Extreme Heat — Delhi — Extreme severity"
3. System identifies Rajesh as covered (Standard plan, covers extreme_heat)
4. **Auto-claim generated**: 10h of lost income (extreme severity) = ₹750 payout
5. AI fraud check runs → Score: 5/100 (low risk) → **Auto-approved**
6. ₹750 sent to Rajesh's UPI within 60 seconds
7. Rajesh receives SMS: "GigShield payout: ₹750 for extreme heat disruption"

### Scenario 2: Mumbai Monsoon Flooding (Amit, Swiggy Rider)

**Profile**: Amit Sharma, bike rider, Andheri zone, 11h/day, ₹5,200/week  
**Event**: Heavy rainfall (95mm/hr) causes waterlogging in Andheri  

**Flow**:
1. Weather API detects 95mm/hr rainfall (threshold: 65mm/hr) → **Severe rain trigger**
2. All Andheri-zone Swiggy riders with rain coverage are identified
3. Auto-claim: 6h lost (severe) = ₹567 payout calculated from Amit's hourly rate
4. Fraud score: 8/100 → **Auto-approved**
5. Instant UPI payout

### Scenario 3: Fraudulent Claim Detection (Kavita, Swiggy Rider)

**Profile**: Kavita Devi, Hyderabad, 10h/day, ₹4,100/week  
**Event**: Kavita's account shows a heat-related claim, but GPS data shows she was in a covered area, and she already filed 3 claims this week  

**Flow**:
1. Claim submitted for extreme heat at 44°C
2. AI fraud detection runs:
   - **High frequency flag** (3 claims in 7 days): +30 points
   - **Duplicate time window** (claim within 1 hour of previous): +25 points
3. Total fraud score: **72/100** → Claim flagged for manual review
4. Admin reviews and either approves or rejects
5. Kavita is notified of the decision

---

## Weekly Premium Model

### Dynamic Pricing (AI-Driven)

GigShield uses **fully dynamic pricing** — premiums are personalized per worker based on risk profile. The tier determines coverage scope, not a fixed price.

| Plan | Typical Weekly Premium | Max Weekly Coverage | Disruptions Covered |
|---|---|---|---|
| **Basic** | ₹20–50/week | 5× premium | Extreme Heat, Heavy Rain |
| **Standard** | ₹40–100/week | 8× premium | + Flood, Pollution |
| **Premium** | ₹60–160/week | 12× premium | + Curfew, Strike |

> **Note**: Premiums shown are ranges, not fixed prices. Each worker gets a personalized rate.

### Premium Calculation Formula

```
Weekly Premium = Base Rate × Avg Weekly Earnings × Risk Multiplier

Where:
- Base Rate = { Basic: 1.5%, Standard: 2.5%, Premium: 4.0% }
- Risk Multiplier = 0.7 + (Risk Score / 100) × 0.6
  (ranges from 0.7× for lowest-risk to 1.3× for highest-risk workers)
```

### Risk Score Calculation (0–100, Capped)

The risk score is a **weighted composite** capped at 100:

```
Risk Score = min(100, Base(50) + City Risk + Vehicle Risk + Hours Risk + Experience Adjustment)
```

| Factor | Weight | Values | Rationale |
|---|---|---|---|
| **Base score** | — | 50 (starting point) | Default moderate risk |
| **City risk** | High | Delhi: +15, Mumbai: +12, Chennai: +10 | Historical disruption frequency per city |
| **Vehicle type** | Medium | Bicycle: +15, Bike: +5, EV: +2 | Vulnerability to weather exposure |
| **Daily hours** | Medium | >10h: +10, >8h: +5 | More outdoor exposure = more risk |
| **Experience** | Adjustable | <6mo: +12, <12mo: +6, >24mo: −8 | New workers have less risk management skill |

**Example 1**: Delhi bike rider, 10h/day, 6 months experience  
→ min(100, 50 + 15 + 5 + 5 + 6) = **Risk Score: 81**  
→ Standard Premium: ₹4,500 × 2.5% × 1.186 = **₹133/week** (max weekly coverage: ₹1,067)

**Example 2**: Pune EV rider, 8h/day, 36 months experience  
→ min(100, 50 + 3 + 2 + 0 − 8) = **Risk Score: 47**  
→ Standard Premium: ₹4,300 × 2.5% × 0.982 = **₹106/week** (max weekly coverage: ₹844)

### Why Weekly?

- Gig workers earn and think in weekly cycles
- Zomato/Swiggy pay riders weekly
- Weekly billing reduces commitment anxiety (vs. monthly/annual)
- Allows dynamic repricing each week as conditions change
- Workers can opt in/out based on seasonal risk (e.g., subscribe during monsoon/summer)

---

## Parametric Triggers

### Trigger Thresholds

| Disruption | Trigger Threshold | Data Source | Severity Levels |
|---|---|---|---|
| **Extreme Heat** | > 42°C | OpenWeatherMap / IMD | Warning (42-44°C), Severe (44-46°C), Extreme (>46°C) |
| **Heavy Rain** | > 65 mm/hr | OpenWeatherMap | Warning (65-80), Severe (80-100), Extreme (>100) |
| **Flood** | Water level > 1.5m | Mock / Municipal APIs | Warning (1.5-2m), Severe (2-3m), Extreme (>3m) |
| **Air Pollution** | AQI > 300 | CPCB / OpenWeatherMap Air | Warning (300-400), Severe (400-500), Extreme (>500) |
| **Curfew** | Section 144 declared | News API / Mock | Binary trigger |
| **Strike** | Zone closure reported | News API / Mock | Binary trigger |

### Personalized Income Loss Calculation

```
Payout = min(Hours Lost × Hourly Rate, Max Weekly Coverage)

Where:
- Hours Lost = min(Worker's Daily Hours × Severity Factor, 12)
- Severity Factor = { Warning: 0.3, Severe: 0.6, Extreme: 1.0 }
- Hourly Rate = Worker's Avg Weekly Earnings / (Avg Daily Hours × 6 working days)
```

**Example**: Worker averages 10h/day, ₹4,500/week  
- Severe event → Hours Lost = min(10 × 0.6, 12) = **6h**  
- Hourly Rate = ₹4,500 / (10 × 6) = **₹75/hr**  
- Payout = min(6 × 75, coverage cap) = **₹450**

### Why Parametric (Not Indemnity)?

| Parametric | Traditional Indemnity |
|---|---|
| Payout based on trigger event | Payout based on actual loss proof |
| Automatic, zero paperwork | Manual claim filing required |
| **2–5 minutes** processing | Weeks to months |
| Objective threshold (42°C) | Subjective assessment |
| Perfect for gig workers | Designed for formal employment |
| Low operational cost | High administrative overhead |

---

## AI/ML Integration Plan

### Phase 1 (Current): Rule-Based AI
- **Risk scoring**: Weighted composite scoring (city risk, vehicle, hours, experience) capped at 100
- **Premium calculation**: Dynamic pricing formula with risk multiplier
- **Fraud detection**: Multi-factor anomaly scoring (7 checks: frequency, duplicates, timing, earnings, policy age, etc.)

### Phase 2 (Weeks 3-4): ML-Enhanced Models

| Model | Purpose | Training Data |
|---|---|---|
| **Gradient Boosted Trees (XGBoost)** | Premium pricing optimization — learns optimal risk→premium mapping from historical claims data | Claims outcomes, weather events, worker profiles |
| **Isolation Forest** | Fraud anomaly detection — identifies unusual claim patterns without labeled fraud data | Claim frequency, amounts, timing, GPS coordinates |

> **Why these two?** XGBoost is the industry standard for insurance pricing (tabular data). Isolation Forest is unsupervised — critical because we don't have labeled fraud datasets yet.

### Phase 3 (Weeks 5-6): Advanced Capabilities

- **GPS trajectory validation**: Compare claimed work area vs. actual location during disruption
- **Predictive risk forecasting**: Next-week disruption probability for dynamic premium adjustment
- **Cross-platform verification**: Match weather data × platform login data × GPS to validate claims

---

## Fraud Detection Architecture

### Multi-Layer Detection

```
Layer 1: Parametric Validation
  └── Does weather data actually confirm the disruption?
      Primary: Open-Meteo API (real-time)
      Fallback: IMD data / OpenWeatherMap
      Validation: Multi-source consensus required for high-value claims

Layer 2: Behavioral Analysis
  ├── Claim frequency (>2/week = capped, >3/week = flagged)
  ├── Duplicate time windows (<1hr between claims)
  ├── Excessive hours claimed (capped at 12h)
  ├── Time-of-day anomaly (midnight-5am claims flagged)
  ├── Earnings anomaly (payout > 80% of weekly earnings)
  └── Policy age check (claims within 48h of creation flagged)

Layer 3: Location Validation (Phase 2)
  ├── GPS vs. claimed zone match
  ├── Platform login activity correlation
  └── Neighboring worker claim patterns
```

### Fraud Score Thresholds

| Score | Action | Rationale |
|---|---|---|
| 0-29 | **Auto-approve** | Low risk, verified parametric event |
| 30-59 | **Queue for review** | Moderate anomalies detected |
| 60-69 | **Flag + hold** | High fraud indicators, manual review required |
| 70-100 | **Flag + block payout** | Very high fraud — payout blocked even if manually approved |

---

## Tech Stack & Architecture

### Frontend
- **React 18** with TypeScript — Component-based SPA
- **Tailwind CSS** — Utility-first responsive styling
- **shadcn/ui** — Accessible, customizable component library
- **Recharts** — Interactive charts for analytics dashboard
- **Wouter** — Lightweight routing

### Backend
- **Node.js + Express** — RESTful API server
- **In-memory storage** (Phase 1 prototype) → **PostgreSQL** (production)
- **Zod** validation — Type-safe request/response schemas

### External APIs
- **Open-Meteo Weather API** — Live temperature, rainfall, humidity for 7 Indian cities (free, no key)
- **Open-Meteo Air Quality API** — Real-time AQI monitoring
- **IMD** — Authoritative fallback for weather data dispute resolution
- **Razorpay Sandbox** — UPI payout simulation (production deployment requires full KYC compliance and RBI payout license)
- **News API** — Curfew/strike detection (Phase 2)

> **Data Reliability Note**: For production insurance payouts, the system implements multi-source consensus — Open-Meteo as primary, IMD as authoritative fallback. Disputes are resolved using IMD's legally recognized data.

### Security Layer
- **Session-based Admin Auth** — SHA-256 password hashing, 8h sessions
- **Rate Limiter** — 30 requests/minute per IP
- **Audit Logger** — All mutations logged with IP, actor, result
- **Weather Cross-Verifier** — Claims verified against live weather data

### Architecture Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    GIGSHIELD PLATFORM                     │
├─────────────┬──────────────┬──────────────┬──────────────┤
│   Worker    │   Policy     │   Claims     │  Analytics   │
│  Onboarding │  Management  │   Engine     │  Dashboard   │
├─────────────┴──────────────┴──────────────┴──────────────┤
│                     API LAYER (Express)                    │
├──────────┬──────────┬───────────┬────────────────────────┤
│ Risk     │ Premium  │ Fraud     │ Parametric             │
│ Engine   │ Calculator│ Detector  │ Trigger Monitor       │
├──────────┴──────────┴───────────┴────────────────────────┤
│                  DATA LAYER                               │
│  Workers | Policies | Claims | Alerts | Payouts           │
├───────────────────────┬──────────────────────────────────┤
│   Weather APIs        │   Payment Gateway (Mock)          │
│   (OpenWeatherMap)    │   (Razorpay Test / UPI Sim)       │
└───────────────────────┴──────────────────────────────────┘
```

---

## Revenue Model

### How GigShield Makes Money

```
Net Revenue = Total Premiums Collected − Claims Paid Out − Operating Costs
```

| Metric | Target |
|---|---|
| **Loss Ratio** | 60–70% (industry standard for parametric insurance) |
| **Operating Expense Ratio** | 15–20% (low due to automation) |
| **Combined Ratio** | <90% (profitable) |
| **Expected Profit Margin** | 10–25% per premium cycle |

**Example** (100 workers on Standard plan, ₹80/week average premium):
- Weekly premium pool: ₹8,000
- Expected claims payout (65% loss ratio): ₹5,200
- Operating costs (18%): ₹1,440
- **Net profit: ₹1,360/week** (17% margin)

### Revenue Scaling Levers
- More workers enrolled → larger premium pool
- AI-optimized pricing → better risk selection
- Lower fraud → reduced claim leakage
- Multi-city expansion → geographic diversification

---

## Development Plan (6-Week Roadmap)

### Phase 1: Ideation & Foundation (Weeks 1-2) ✅ CURRENT

**Status: Complete**

- [x] Problem research and persona selection
- [x] Weekly premium model design
- [x] Parametric trigger definition
- [x] AI risk scoring algorithm
- [x] Fraud detection framework
- [x] Full-stack prototype with:
  - Admin dashboard with KPI cards, charts, alerts
  - Worker registration with AI risk profiling
  - Policy creation with dynamic premium calculation
  - Claims management with fraud scoring
  - Weather alert monitoring
  - Disruption simulation engine
- [x] **Live weather integration** (Open-Meteo API, 7 cities)
- [x] **Admin authentication** (session-based, rate-limited)
- [x] **Anti-fraud protections** (24h cooldown, weekly caps, weather verification)
- [x] **Enhanced fraud detection** (7 scoring factors)
- [x] **Audit logging** (all admin actions tracked)
- [x] **Error boundaries** (graceful crash recovery)
- [x] README documentation
- [x] GitHub repository

### Phase 2: Automation & Protection (Weeks 3-4)

- [ ] OpenWeatherMap API integration (live weather data)
- [ ] Real-time parametric trigger monitoring (polling every 5 min)
- [ ] Worker-facing mobile-responsive portal
- [ ] Registration flow with OTP verification
- [ ] Policy purchase with Razorpay test mode
- [ ] 3-5 automated triggers with real API data
- [ ] ML-based premium optimization (XGBoost)
- [ ] Claims auto-processing pipeline
- [ ] Push notifications for alerts and payouts
- [ ] Demo video (2 min)

### Phase 3: Scale & Optimise (Weeks 5-6)

- [ ] Advanced fraud detection (GPS validation, Isolation Forest)
- [ ] Predictive analytics dashboard (next-week risk forecasting)
- [ ] Worker dashboard (earnings protected, active coverage, claim history)
- [ ] Admin dashboard (loss ratios, claim analytics, fraud stats)
- [ ] Razorpay sandbox payout integration
- [ ] Performance optimization and load testing
- [ ] Final pitch deck
- [ ] 5-minute demo video with simulated disruption walkthrough

---

## Platform Choice Justification

### Why Web (Not Mobile)?

| Factor | Web | Mobile |
|---|---|---|
| Development speed | Faster for hackathon timeline | Slower (native for both iOS/Android) |
| Accessibility | Works on any device with a browser | Requires app download |
| Deployment | Instant updates, no app store review | Play Store review delays |
| Admin dashboard | Natural fit for desktop web | Poor fit for mobile |
| Worker access | Mobile-responsive web works well | Better native UX |

**Decision**: Build a **responsive web app** that works well on both desktop (admin) and mobile (worker).  
**Phase 2 consideration**: If needed, wrap in a PWA or React Native for app-like mobile experience.

---

## Prototype Status

### What's Built (Phase 1 Prototype)

The working prototype demonstrates:

1. **Admin Dashboard** — KPI cards (workers, policies, claims, payouts), weekly premiums vs claims chart, claims by disruption type donut chart, active weather alerts, recent claims feed
2. **Worker Management** — Registration form with platform/city/zone/vehicle selection, AI risk score calculation on registration
3. **Policy Management** — Create policies with tier selection (Basic/Standard/Premium), dynamic premium calculator showing risk score and weekly price
4. **Claims Management** — Full claims table with fraud scoring, auto-approval for low-risk claims, manual approve/reject for flagged claims, instant UPI payout processing
5. **Weather Alerts** — Active alert cards with severity levels, threshold reference table, resolve functionality
6. **Disruption Simulator** — Quick scenario presets (Heatwave in Delhi, Rain in Mumbai, etc.), custom trigger configuration, full pipeline visualization (Alert → Workers Found → Claims Generated → Fraud Scored → Auto-Approved)

### Live Demo

The prototype is deployed and accessible. All features use realistic seed data with 8 delivery workers across 7 Indian cities, covering all three platforms (Zomato, Swiggy, Zepto). Live weather data is fetched from Open-Meteo API.

---

## How to Run Locally

```bash
# Clone the repository
git clone https://github.com/ABHISHEK1139/gigshield-devtrails-2026
cd gigshield-devtrails-2026

# Install dependencies
npm install

# Run development server
npm run dev

# Access at http://localhost:5000
```

### Default Admin Credentials

| Field | Value |
|---|---|
| Username | `admin` |
| Password | `gigshield2026` |

### Environment Variables

| Variable | Default | Description |
|---|---|---|
| `PORT` | `5000` | Server port |
| `NODE_ENV` | `development` | Set to `production` to disable simulation endpoint |

---

## API Reference

### Auth Endpoints (No Auth Required)
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/login` | Login with username/password |
| POST | `/api/auth/logout` | End admin session |
| GET | `/api/auth/session` | Check current session |

### Read-Only Endpoints (No Auth Required)
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/dashboard` | Dashboard statistics |
| GET | `/api/workers` | List all workers |
| GET | `/api/policies` | List all policies |
| GET | `/api/claims` | List all claims |
| GET | `/api/alerts` | Active weather alerts |
| GET | `/api/weather/live` | Live weather for all cities |
| GET | `/api/weather/city/:city` | Weather for a specific city |

### Admin-Only Endpoints (Requires Bearer Token)
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/workers` | Register a new worker |
| POST | `/api/policies` | Create a policy |
| POST | `/api/claims` | Submit a claim (with weather verification) |
| PATCH | `/api/claims/:id/status` | Approve/reject/pay a claim |
| POST | `/api/payouts` | Process a payout |
| POST | `/api/alerts` | Create a weather alert |
| PATCH | `/api/alerts/:id/resolve` | Resolve an alert |
| POST | `/api/weather/check-triggers` | Check live weather for threshold breaches |
| POST | `/api/simulate-trigger` | Simulate a disruption (dev only) |
| GET | `/api/audit-log` | View admin action log |

---

## Data Sources & References

1. Moneycontrol (Jan 2026) — [Delivery Partner Earnings Analysis](https://www.moneycontrol.com/news/business/startup/how-much-do-delivery-partners-actually-earn-a-look-inside-the-pay-model-for-gig-workers-on-zomato-swiggy-13756512.html)
2. EfD Initiative — [Rising temperatures cause lost incomes for informal workers](https://www.efdinitiative.org/about-efd/efd-reports/efd-annual-report-2024/impact-stories/rising-temperatures-cause-lost-incomes)
3. Down To Earth (Nov 2025) — [India's Gig Workforce at Boiling Point](https://www.downtoearth.org.in/climate-change/riders-in-the-heat-indias-gig-workforce-at-boiling-point)
4. NPR (Jul 2025) — [Parametric Insurance for Gig Workers in India](https://www.npr.org/sections/goats-and-soda/2025/07/16/g-s1-76948/hot-temperatures-insurance-gig-workers-india)
5. J-PAL — [Parametric Insurance and Gig Worker Labor Supply](https://www.povertyactionlab.org/initiative-project/take-and-impacts-parametric-insurance-labor-supply-under-climate-change)
6. TGPWU Survey (2024) — [Impact of Extreme Heat on Gig Workers](https://tgpwu.org/wp-content/uploads/2024/08/Impact_of_Extreme_Heat_on_Gig_Workers_A_Survey_Report-1.pdf)
7. Guidewire Blog — [Combating AI-Generated Media Fraud in Insurance Claims](https://www.guidewire.com/resources/blog/industry-trends/combating-ai-generated-media-fraud-in-insurance-claims)
8. Deloitte Insights — [Using AI to Fight Insurance Fraud](https://www.deloitte.com/us/en/insights/industry/financial-services/financial-services-industry-predictions/2025/ai-to-fight-insurance-fraud.html)
9. OpenWeatherMap — [Weather API Documentation](https://openweathermap.org/api)
10. Open-Meteo — [Free Open-Source Weather API](https://open-meteo.com)

---

## License

MIT License — Guidewire DEVTrails 2026 Hackathon Submission
