# GigSafe — Parametric Income Insurance for Gig Workers

> **Guidewire DEVTrails 2026 | Team Submission | Phase 1: Ideation & Foundation**

---

## Team

| Name | Role |
|---|---|
| Abhinav Raj | Frontend & Design |
| Nouman Shafique | Backend & DevOps |
| Nandini Yadav | Product & Design |
| Ishan Bag | AI/ML & Integrations |

---

## Problem Statement

India's Q-Commerce delivery partners (Zepto, Blinkit, Swiggy Instamart, BigBasket Now) are the engine of the instant-delivery economy. These workers operate on tight margins — a typical delivery partner earns ₹600–900/day working 8–10 hour shifts. However, external disruptions such as extreme rainfall, heatwaves, hazardous air quality, and local curfews or strikes can bring deliveries to a complete halt, causing workers to lose 20–30% of their monthly income with zero safety net.

**GigSafe** solves this by providing an AI-enabled parametric income insurance platform built specifically for Q-Commerce delivery partners. When a qualifying disruption is detected, claims are triggered automatically — no paperwork, no waiting, no fraud. The worker receives their lost income payout directly within hours.

---

## Why Parametric Insurance — Not Traditional Insurance

Traditional insurance requires a worker to: (1) experience a loss, (2) file a claim with documentation, (3) wait for an investigator to verify the claim, and (4) receive a payout weeks later. For a gig worker earning ₹700/day, this process is completely unusable. They cannot afford to wait weeks, they have no documentation of daily income, and the overhead of filing a claim is higher than the payout itself.

**Parametric insurance eliminates all of this.** Instead of compensating for a verified, documented loss, it pays out automatically when a pre-agreed objective trigger condition is met — regardless of what actually happened to the individual worker. If rainfall at a pincode exceeds 35mm/hr for one hour, every insured worker in that zone is paid. No claim. No investigator. No wait.

This model is uniquely suited to gig workers because:
- **The trigger is objective** — pulled from third-party APIs (IMD, OpenWeatherMap, CPCB), not self-reported by the worker. This eliminates the most common fraud vector in traditional insurance.
- **The payout is instant** — funds reach the worker's UPI account within hours of the disruption, not weeks. This matches the financial reality of workers living on weekly earnings.
- **The product is simple** — a worker doesn't need to understand policy terms, excess clauses, or documentation requirements. They pay ₹25–50/week and get money when the weather turns bad. That's it.

GigSafe is built entirely on this parametric model. There is no manual claim filing in our system at any point. We have defined **4 parametric triggers** (extreme rainfall, extreme heat, severe air pollution, and local curfews/strikes), each with precise, measurable thresholds pulled from independent third-party data sources.

---

## Persona: Q-Commerce Delivery Partner

**Who we serve:** Delivery partners working for Zepto, Blinkit, Swiggy Instamart, and BigBasket Now across pan-India metros — Bengaluru, Mumbai, Delhi NCR, Hyderabad, Pune, and Chennai.

**Profile:**
- Age: 20–40 years
- Income: ₹500–900/day, paid or tracked on a weekly cycle
- Device: Android smartphone (primary tool for order acceptance)
- Financial literacy: Low to moderate — needs zero-friction UX
- Pain point: No income safety net during weather/civic disruptions

### Persona Scenarios

**Scenario 1 — Heavy Monsoon, Bengaluru**
Raju is a Zepto delivery partner in Koramangala, Bengaluru. On a Tuesday evening, the IMD rainfall index crosses 40mm/hr. The platform suspends deliveries for his zone. He loses a 4-hour prime earning window — approximately ₹280 in lost wages. GigSafe detects the rainfall trigger, auto-initiates a claim, and credits ₹280 to his linked UPI account within 2 hours. No action required from Raju.

**Scenario 2 — Extreme Heat, Delhi NCR**
Priya is a Blinkit partner in Noida. In May, a heatwave pushes the temperature to 46°C for 3 consecutive hours between 1–4 PM. Platform order volumes drop by 70%. GigSafe's heat trigger fires, and Priya receives a proportional payout for the disrupted window without filing any manual claim.

**Scenario 3 — Local Strike / Curfew, Mumbai**
Suresh operates in Dharavi, Mumbai. An unannounced transport union strike blocks road access to his pickup zone for 6 hours. GigSafe's civic disruption module detects a government-issued curfew notification in his registered delivery zone and automatically triggers a payout covering his estimated lost shift earnings.

**Scenario 4 — Severe Pollution, Delhi NCR**
Anjali is a Swiggy Instamart partner in Anand Vihar, Delhi. The AQI spikes to 380 (Severe category) for 4+ consecutive hours. Outdoor delivery activity is restricted. GigSafe's AQI trigger fires and she is compensated for the period she could not work.

---

## Coverage Scope

GigSafe covers **income lost due to qualifying external disruptions only.**

### Covered Disruptions

| Disruption Type | Trigger Condition | Data Source |
|---|---|---|
| Extreme Rainfall / Flood | Rainfall ≥ 35mm/hr at worker's registered pincode for ≥ 1 hour | OpenWeatherMap API |
| Extreme Heat | Temperature ≥ 44°C for ≥ 3 consecutive hours | OpenWeatherMap API |
| Severe Air Pollution | AQI ≥ 300 (Severe category) for ≥ 3 consecutive hours | CPCB / AQI India API |
| Local Curfew / Strike | Verified govt curfew or civic disruption alert in worker's delivery zone | Govt notification feeds / mock |

### Explicitly Excluded (Hard Constraints)
- ❌ Health insurance or medical expenses
- ❌ Accident coverage or hospitalization
- ❌ Vehicle repair, fuel, or maintenance
- ❌ Life insurance
- ❌ Income loss due to personal reasons (illness, leave)

---

## Application Workflow

### Platform Choice: Mobile App (Flutter)

We chose a **mobile-first Android application** as our primary platform for three concrete reasons. First, Q-Commerce delivery partners in India use Android smartphones as their primary work tool — every order acceptance, navigation, and platform communication happens on their phone. A mobile app means GigSafe lives exactly where they already work, requiring zero behaviour change. Second, a native Flutter app gives us direct access to device-level signals — GPS, accelerometer, network state, and app activity — that are critical for our fraud detection and location validation logic. A web app cannot access these sensors. Third, our persona has low digital literacy and limited time; a mobile app with push notifications for claim updates and one-tap policy renewal fits their workflow far better than a browser-based tool. A web dashboard is separately provided for the insurer and admin side, where detailed analytics and claim review workflows are better suited to a larger screen.

### Worker-Facing Flow

```
1. ONBOARDING
   Worker downloads GigSafe app
   → Registers with phone number (OTP verification)
   → Selects delivery platform (Zepto / Blinkit / Swiggy Instamart etc.)
   → Declares weekly income & working hours
   → Selects home delivery zone (pincode / city)
   → AI Risk Engine generates a personalised weekly premium (₹25–50)
   → Worker activates weekly policy via Razorpay payment

2. ACTIVE COVERAGE
   Policy is live for the week
   → GigSafe monitors weather, AQI, and civic alert feeds in real time
   → Worker's GPS zone is validated against incoming disruption signals

3. DISRUPTION DETECTED (Parametric Trigger Fires)
   System detects qualifying disruption in worker's registered zone
   → Claim auto-initiated — no worker action needed
   → AI fraud engine validates: location match, no duplicate, not a pre-existing event
   → Claim approved or flagged for review

4. PAYOUT
   Approved claim triggers instant payout via Razorpay (UPI / bank transfer)
   → Worker notified via push notification + SMS
   → Earnings protection summary updated on dashboard

5. POLICY RENEWAL
   Every Sunday, worker is prompted to renew for the next week
   → Premium dynamically recalculated based on updated risk data
```

### Insurer / Admin-Facing Flow (Web Dashboard)

```
Admin Dashboard
→ View all active policies, claims, payout history
→ Monitor live disruption events across zones
→ Review flagged claims (fraud-suspected cases)
→ Approve / reject edge-case claims manually
→ Analytics: loss ratios, claim frequency, zone-level risk heatmap
→ Predictive analytics: next week's likely high-risk zones
```

---

## Weekly Premium Model

### How It Works

GigSafe's premium is structured on a **weekly basis** to align with the gig worker's weekly earning cycle. The premium is not fixed — it is dynamically calculated by our AI risk engine for each worker each week.

**Base Premium Range: ₹25 – ₹50 per week**

### Premium Calculation Inputs

| Factor | Description | Impact |
|---|---|---|
| Delivery Zone Risk Score | Historical disruption frequency for worker's pincode | Primary driver |
| Platform | Different platforms have different order-suspension thresholds | ±10% |
| Declared Weekly Income | Higher income = higher coverage = higher premium | Proportional |
| Season / Month | Monsoon months get higher base risk | +15–25% |
| Worker Claim History | Frequent past claims increase premium slightly | ±5–15% |
| Coverage Tier | Basic (₹400 payout cap) / Standard (₹750 cap) / Premium (₹1,000 cap) | Tier-based |

### Example

> Raju, Zepto partner, Koramangala (Bengaluru) — July (peak monsoon)
> - Zone Risk Score: High (7.2/10 — historically flood-prone)
> - Declared weekly income: ₹3,500
> - Coverage tier: Standard (up to ₹750/week)
> - **Calculated weekly premium: ₹42**

### Parametric Triggers & Payout Logic

When a trigger fires, the payout is calculated as:

```
Payout = min(Declared Daily Income × Disrupted Hours / 8, Weekly Coverage Cap)
```

**Coverage cap: Up to ₹750 per week** across all qualifying disruption events.

| Trigger | Payout Rate |
|---|---|
| Rainfall ≥ 35mm/hr for 1–3 hrs | ₹100–₹250 |
| Rainfall ≥ 35mm/hr for 3+ hrs | ₹250–₹500 |
| Temperature ≥ 44°C for 3–5 hrs | ₹150–₹300 |
| AQI ≥ 300 for 3–5 hrs | ₹150–₹300 |
| Curfew / Strike (zone-level) | Up to ₹500/event |

Payouts in a single week are capped at ₹750 regardless of stacking events.

---

## AI / ML Integration Plan

### 1. Dynamic Premium Calculation — scikit-learn Risk Model

We will train a **regression model using scikit-learn** (likely Gradient Boosting Regressor / XGBoost) to predict a personalised risk score per worker per week.

**Training Data Sources:**
- Historical weather data (IMD / OpenWeatherMap historical API) by pincode
- Historical claim frequency data (seeded/synthetic for Phase 1, real data in Phase 3)
- Seasonal disruption calendars (monsoon, summer heatwave windows)
- Zone-level delivery drop-off data (mocked for Phase 1)

**Model Inputs:** Worker pincode, platform, season, declared income, historical claim rate, current week weather forecast risk  
**Model Output:** Risk score (0–10) → mapped to premium (₹25–₹50)

**Phase 1 scope:** Rule-based scoring with the ML model architecture defined. Actual trained model integrated in Phase 2.

### 2. Fraud Detection

GigSafe's fraud engine validates every auto-initiated claim against the following signals:

| Signal | What We Check |
|---|---|
| Location Validation | Worker's live GPS at time of trigger matches registered delivery zone |
| Event Authenticity | Disruption data from official API — not self-reported |
| Duplicate Prevention | One claim per disruption event per worker |
| Temporal Validation | Claim window matches the exact disruption window |
| Behavioural Anomaly | Unusual claim frequency patterns flagged for manual review |
| Platform Activity Cross-check | If platform API shows orders were accepted during disruption, claim is flagged |

**Phase 2+:** Anomaly detection model (Isolation Forest / Autoencoder) trained on claim patterns to flag statistical outliers.

### 3. Additional AI Features (Phase 3)
- **Predictive Risk Alerts:** Notify workers 24h in advance of high-risk weather windows so they can activate coverage proactively
- **NLP Claim Assistant:** In-app chatbot to handle edge-case claim queries
- **Zone Risk Heatmap:** Admin dashboard visual showing predicted high-claim zones for next week

---

## Tech Stack

### Core Stack

| Layer | Technology | Reason |
|---|---|---|
| Mobile Frontend | Flutter (Dart) | Cross-platform (Android priority), single codebase, fast UI |
| Backend / API | Python + FastAPI | Lightweight, async-ready, excellent ML library ecosystem |
| Database | Supabase (PostgreSQL) | Managed Postgres + built-in Auth + Realtime subscriptions |
| AI / ML | scikit-learn, XGBoost, pandas, numpy | Risk scoring, fraud detection |
| Authentication | Supabase Auth (OTP via phone) | Frictionless for low-literacy users |
| Push Notifications | Firebase Cloud Messaging (FCM) | Standard for Android delivery |
| Payments / Payouts | Razorpay (Test Mode) | UPI, IMPS, bank transfer — widely used in India |

### Third-Party API Integrations

| API | Purpose | Mode |
|---|---|---|
| OpenWeatherMap API | Real-time rainfall & temperature triggers | Free tier (live) |
| AQI India / CPCB API | Air Quality Index triggers | Free tier / mock |
| Google Maps Geocoding API | Pincode-to-zone validation | Free tier |
| Razorpay Payment Gateway | Premium collection & claim payouts | Test/sandbox mode |
| Firebase Cloud Messaging | Push notifications to workers | Free tier |
| Govt Alert Feed (mock) | Curfew / strike zone notifications | Mocked for Phase 1 |

### High-Level System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        GigSafe Platform                      │
│                                                             │
│  ┌──────────────┐    ┌──────────────────┐    ┌───────────┐ │
│  │  Flutter App  │───▶│  FastAPI Backend  │───▶│ Supabase  │ │
│  │  (Worker)     │    │                  │    │ PostgreSQL│ │
│  └──────────────┘    │  ┌────────────┐  │    └───────────┘ │
│                       │  │ AI/ML      │  │                  │
│  ┌──────────────┐    │  │ Risk Engine│  │    ┌───────────┐ │
│  │  Web Dashboard│───▶│  └────────────┘  │───▶│  Razorpay │ │
│  │  (Admin)     │    │                  │    │  Payouts  │ │
│  └──────────────┘    │  ┌────────────┐  │    └───────────┘ │
│                       │  │ Trigger    │  │                  │
│                       │  │ Monitor    │◀─┼── External APIs  │
│                       │  └────────────┘  │    (Weather/AQI) │
│                       └──────────────────┘                  │
└─────────────────────────────────────────────────────────────┘
```

---

## Development Plan

### Phase 1 (Mar 4–20): Ideation & Foundation ✅
- [x] Define persona, scenarios, and disruption coverage scope
- [x] Design weekly premium model and parametric trigger logic
- [x] Finalise tech stack and system architecture
- [x] Set up GitHub repository and project structure
- [x] Build Figma wireframes for worker onboarding and dashboard screens
- [x] Record 2-minute strategy + prototype walkthrough video

### Phase 2 (Mar 21–Apr 4): Automation & Protection
- [ ] Worker registration and OTP-based onboarding (Flutter)
- [ ] Policy creation with dynamic premium calculation (FastAPI + scikit-learn)
- [ ] Supabase schema: workers, policies, claims, payouts tables
- [ ] Real-time trigger monitoring module (OpenWeatherMap + AQI API polling)
- [ ] Auto claim initiation engine — 3–5 parametric triggers live
- [ ] Razorpay sandbox integration — premium payment + payout simulation
- [ ] Basic fraud validation (location check, duplicate prevention)
- [ ] 2-minute demo video

### Phase 3 (Apr 5–17): Scale & Optimise
- [ ] Advanced fraud detection model (Isolation Forest on claim patterns)
- [ ] GPS spoofing detection
- [ ] Worker dashboard: earnings protected, active coverage, claim history
- [ ] Admin dashboard: loss ratios, zone-level heatmaps, predictive analytics
- [ ] Instant payout simulation (Razorpay test mode end-to-end flow)
- [ ] Final pitch deck (PDF)
- [ ] 5-minute full walkthrough demo video

---

## Current Prototype Scope (Phase 1)

As of the Phase 1 submission, the following has been completed:

- ✅ Figma wireframes for worker onboarding, active policy view, claim notification, and payout screen
- ✅ System architecture diagram and database schema design
- ✅ GitHub repository initialised with project structure
- ✅ Weekly premium calculation logic defined and rule-based prototype built
- ✅ Parametric trigger thresholds defined with data source mapping

**2-minute strategy and prototype video:** [Link to be added]  
**GitHub Repository:** [Link to be added]

---

---

## Adversarial Defense & Anti-Spoofing Strategy

### The Threat

A coordinated syndicate of bad actors using GPS-spoofing applications can fake their location into a red-alert weather zone while sitting safely at home, triggering mass false payouts and draining the liquidity pool. Simple GPS coordinate verification is insufficient — a spoofed GPS signal is indistinguishable from a real one at the coordinate level alone.

GigSafe's defense operates across three layers: **AI/ML differentiation**, **multi-signal data fusion**, and **a human-safe flagging workflow** that protects honest workers.

---

### 1. Differentiation — How We Tell a Genuine Worker from a Spoofer

A GPS spoofer can fake coordinates, but they cannot simultaneously fake every other data signal that a genuinely active delivery worker generates. GigSafe's fraud engine builds a **Behavioural Consistency Score (BCS)** for each claim by cross-referencing GPS against a fingerprint of corroborating signals.

| Signal | Genuine Worker Behaviour | Spoofer Behaviour |
|---|---|---|
| **Accelerometer / Motion data** | Shows movement patterns consistent with travel — starts, stops, turns, vibration of a vehicle or bike | Stationary or perfectly smooth — no micro-vibrations, no stop-start motion |
| **Network cell tower triangulation** | Device is connecting to cell towers geographically consistent with the claimed GPS zone | Device connects to towers in a different area than the claimed GPS coordinates |
| **Battery & screen activity** | Phone is actively used — screen on/off, app switching, navigation open | Phone may be idle or running a single spoofing app in the background |
| **App activity context** | Delivery platform app (Zepto/Blinkit) is active or was recently active on the device | Delivery platform app is closed or inactive during the claim window |
| **Historical GPS trajectory** | Location history shows a coherent travel path entering and leaving the disruption zone | Location jumps instantly to the zone with no approaching trajectory |
| **Claimed zone vs home zone** | Worker's registered delivery zone matches the disruption pincode | Worker's registered zone is in a different part of the city |

The BCS is a weighted score (0–100) calculated from the above signals at claim time. Claims with a BCS below 60 are automatically flagged for review rather than auto-approved.

---

### 2. Coordinated Ring Detection — Data Points Beyond GPS

Individual spoofers are detectable. A **coordinated syndicate of 500 workers** generates a statistical signature that is even easier to catch at the population level.

**Signals we monitor for coordinated fraud rings:**

- **Claim velocity spike:** If the number of claims from a single pincode jumps from a baseline of 5/hour to 200/hour within minutes of a trigger, this is a statistical anomaly. Genuine disruptions cause gradual claim upticks; coordinated fraud causes sudden vertical spikes.
- **Device fingerprint clustering:** Multiple claims originating from devices sharing the same Wi-Fi router IP address, the same hardware model batch, or devices that have never previously had the delivery platform app active.
- **Telegram / social coordination signal (metadata only):** Abnormal spikes in new policy sign-ups immediately preceding a weather event in a zone — indicating advance knowledge of an upcoming trigger, possibly spread via group chats.
- **Identical claim timing:** In a genuine disruption, workers file claims spread across the disruption window. Coordinated rings tend to file claims in a burst within seconds of the trigger firing, indicating automated or scripted claim initiation.
- **Geofence inconsistency at scale:** If 90%+ of claimants in a zone show stationary accelerometer data simultaneously, this is statistically impossible for a genuine disruption scenario where workers would be in varied states of movement or evacuation.

These signals feed into an **Isolation Forest anomaly detection model** (Phase 2+) trained on historical claim patterns to assign a Ring Fraud Probability score per cluster of simultaneous claims.

---

### 3. UX Balance — Protecting Honest Workers Who Get Flagged

The most important constraint: **a genuine worker in a bad weather zone may have a weak GPS signal, a low-battery phone, or a network drop — and they must not be penalised for it.**

GigSafe's flagging workflow is designed to be innocent-until-proven-guilty:

```
CLAIM FLAGGED (BCS < 60 or Ring Fraud Probability > 0.7)
       │
       ▼
STEP 1 — Soft Hold (not rejection)
   Payout is held for up to 6 hours, not rejected.
   Worker receives a push notification:
   "Your claim is under a quick review. You'll hear from us within 6 hours."
       │
       ▼
STEP 2 — Passive Re-verification (no worker action needed)
   System re-checks signals over the next 2 hours:
   → Did the worker's cell tower data update to match the claimed zone?
   → Did motion data change (e.g. worker started moving after shelter)?
   → Did the delivery platform app become active?
   If signals improve → BCS recalculated → auto-approved if above threshold.
       │
       ▼
STEP 3 — One-tap Soft Confirmation (only if still flagged)
   If BCS remains low after passive re-verification:
   Worker is shown a single in-app screen:
   "We noticed unusual activity on your claim. Tap to confirm you were
    working in [Zone Name] during [Time Window]."
   → One tap. No documents. No explanation required.
   → Honest workers confirm in seconds.
   → Bad actors in a ring will either not respond or confirm falsely
     (which adds to their fraud risk score for future claims).
       │
       ▼
STEP 4 — Admin Review Queue (only for persistent anomalies)
   Claims that fail all three steps above go to the admin dashboard
   for manual review by the insurer.
   → Admin sees the full BCS breakdown, motion data summary, and
     ring fraud probability score.
   → Target: fewer than 2% of genuine claims reach this step.
```

**Key design principle:** At no point does a flagged worker lose their claim automatically. The worst outcome for an honest worker is a 6-hour delay, not a rejection. Rejections only happen after manual admin review with full evidence.

---

## Fault Tolerance & Resilience Architecture

GigSafe's core promise to workers — *"you will be paid within 2 hours of a qualifying disruption"* — is only as strong as the system's ability to keep running when individual components fail. Because the platform depends on multiple third-party APIs, a payment gateway, and real-time data feeds, every failure mode has been explicitly designed for. There are no silent failures in GigSafe.

---

### 1. External API Failures (Weather / AQI / Govt Feeds)

The trigger monitoring pipeline is the most failure-prone layer because it depends on external services GigSafe does not control.

**Strategy: Multi-source redundancy with automatic fallback**

| Primary Source | Fallback Source | Behaviour on Failure |
|---|---|---|
| OpenWeatherMap API | IMD (India Meteorological Department) data feed | Automatic switch; alert logged to admin dashboard |
| CPCB AQI API | AQI India public API | Automatic switch; no service interruption |
| Govt alert feed | Manual admin input + verified news feed scraper | Degraded mode; admin notified to verify manually |

**Polling resilience:** The trigger monitor uses exponential backoff on API failures — retrying after 30s, 60s, 2min, 5min before escalating to the fallback source. If both primary and fallback fail simultaneously, the trigger monitor enters a **safe hold state**: no new triggers fire, active policies remain valid, and the monitoring window is extended by the duration of the outage. Workers are never penalised for outages they cannot see.

**Stale data detection:** Every API response is timestamped. If the trigger monitor receives data older than 15 minutes, it treats the feed as degraded and switches to fallback immediately rather than acting on outdated readings.

---

### 2. Payout Failures (Razorpay / UPI)

A failed payout is the worst possible outcome for a worker. GigSafe treats payout reliability as a first-class requirement.

**Strategy: Persistent retry queue with dead-letter escalation**

```
PAYOUT INITIATED
       │
       ▼
Razorpay API call
       │
   ┌───┴────┐
Success    Failure
   │           │
   ▼           ▼
Mark paid   Add to retry queue
            → Retry after 5 min
            → Retry after 15 min
            → Retry after 1 hr
                    │
               Still failing?
                    │
                    ▼
            Dead-letter queue
            → Admin alerted immediately
            → Manual payout initiated
            → Worker notified:
              "Payment delayed — arriving within 6 hours"
```

All payout attempts are persisted to the Supabase `payout_attempts` table **before** the API call is made. If the backend crashes mid-payout, the record survives and the retry worker picks it up on restart. **Payouts are never lost — they are either completed or queued.**

**Idempotency:** Every Razorpay call uses a unique idempotency key (`claim_id + attempt_number`). If the network drops after Razorpay processes a payment but before GigSafe receives the confirmation, a retry will not double-pay the worker — Razorpay returns the original successful response.

---

### 3. Backend Failures (FastAPI Service)

**Strategy: Stateless services + database-as-source-of-truth**

The FastAPI backend is designed stateless — all claim state, policy state, and payout state lives in Supabase PostgreSQL, never in application memory. A crashed backend process can be restarted instantly with no data loss and no in-progress claim corruption.

**Crash recovery behaviour:**
- In-flight claim evaluations are re-queued on startup by scanning for claims with status `evaluating` older than 2 minutes.
- In-flight payouts are re-queued by scanning for `payout_attempts` with status `pending` older than 10 minutes.
- The trigger monitor re-initialises its polling loop from the last successful timestamp stored in the database.

**Deployment:** The FastAPI service runs with a minimum of 2 replicas behind a load balancer. If one replica crashes, traffic routes to the surviving replica within seconds. Health checks run every 10 seconds; an unhealthy replica is replaced automatically.

---

### 4. Database Failures (Supabase / PostgreSQL)

Supabase provides managed PostgreSQL with automatic daily backups and point-in-time recovery. GigSafe additionally implements:

- **Write-ahead logging (WAL):** All critical tables (policies, claims, payouts) use PostgreSQL WAL, ensuring no committed transaction is ever lost even if the database crashes immediately after a write.
- **Read replica for analytics:** The admin dashboard and risk engine read from a Supabase read replica, isolating analytical queries from the transactional write path. A slow analytics query cannot block a claim write.
- **Connection pooling via PgBouncer:** Supabase's built-in PgBouncer prevents connection exhaustion during claim surge events — e.g., 500 workers filing claims simultaneously after a city-wide rainfall trigger.

---

### 5. Mobile App Offline Resilience

Workers in active disruption zones may lose network connectivity at the worst possible moment — heavy rain degrades mobile signals significantly. The Flutter app handles this explicitly.

**Offline behaviour:**
- The app caches the worker's current policy details, coverage status, and last known trigger state locally using `Hive` (local Flutter storage).
- If the app cannot reach the GigSafe backend, it displays the last known policy status with a "Last updated: X mins ago" indicator rather than showing an error screen.
- Push notifications (FCM) are delivered with a 72-hour TTL — if the worker's phone is offline when a claim is approved, the notification is delivered when connectivity resumes.
- Premium payments queue locally if the worker attempts renewal while offline, retrying automatically when the network is restored.

---

### 6. Surge Handling — Mass Simultaneous Claims

A city-wide rainfall event can trigger thousands of claims within minutes. This is the expected peak-load scenario, not a failure — but it must not degrade payout speed or accuracy.

**Strategy: Async claim processing with a durable job queue**

1. The trigger monitor publishes a `disruption_event` to a **Redis-backed job queue** (Phase 2 implementation).
2. A pool of claim worker processes consumes from the queue and evaluates claims in parallel.
3. The Flutter app receives an immediate acknowledgement ("Claim initiated — processing") and is updated via Supabase Realtime when the claim resolves.
4. The queue is durable — if a worker process crashes mid-job, the job is re-queued and picked up by another worker automatically.

**Rate limiting on Razorpay:** In a mass payout event, GigSafe batches payouts using Razorpay's bulk transfer API rather than firing individual API calls per worker, staying within rate limits while maintaining the 2-hour SLA.

---

### 7. Insurance-Specific Fault Detection

Beyond infrastructure resilience, GigSafe monitors for faults specific to the insurance domain — situations where the system is technically running but producing incorrect insurance outcomes.

| Fault Type | Detection Mechanism | Response |
|---|---|---|
| **Trigger misfiring** (payout issued when threshold not actually met) | Every trigger event is logged with the raw API value that crossed the threshold. Nightly reconciliation job cross-checks fired triggers against historical API data. | Misfired payouts flagged for admin review; model recalibrated. |
| **Trigger not firing** (threshold met but no payout issued) | Watchdog job scans for zones where API data shows threshold-crossing conditions but no claim events were generated within 10 minutes. | Admin alerted; affected workers identified and manually compensated. |
| **Premium calculation drift** (ML model producing anomalous premiums) | Automated monitoring checks that generated premiums fall within ₹15–₹100 range. Outliers beyond 2 standard deviations from weekly mean are flagged. | Anomalous premium held for review; default rule-based premium applied in the interim. |
| **Double payout** (same worker paid twice for same event) | Idempotency key enforcement at Razorpay level + unique constraint on `(worker_id, claim_id)` in the payouts table. | Structurally prevented — cannot occur by design. |
| **Stale policy payout** (worker's policy expired but claim was processed) | Policy validity window checked at claim initiation time using a database-level `valid_until` timestamp comparison, not application-level logic. | Expired policy claims rejected at DB layer; cannot be bypassed by application bugs. |
| **Zone boundary error** (worker in adjacent zone gets paid for wrong zone trigger) | Trigger events are matched to workers using pincode-level geofencing, not city-level. Worker's registered pincode must fall within the trigger's affected pincode list. | Granularity of pincode matching prevents cross-zone misfiring. |

---

### 8. Fault Tolerance Summary

| Failure Scenario | Detection | Recovery | Worker Impact |
|---|---|---|---|
| Primary weather API down | Stale data timestamp check (>15 min) | Auto-switch to fallback API | None — seamless |
| All weather APIs down | Two consecutive fallback failures | Safe hold state; outage window extended | Trigger window paused, not lost |
| Razorpay payout failure | HTTP error / timeout on API call | Retry queue → dead-letter → manual payout | Delay up to 6 hrs; never lost |
| Backend crash mid-claim | Stale `evaluating` status on startup scan | Auto re-queue on restart | Claim delayed by restart time only |
| Database connection loss | PgBouncer health check | Connection re-pooled; WAL protects writes | Transient; sub-second recovery |
| Worker phone offline | App detects no connectivity | Cached policy shown; FCM TTL 72h | Notification delayed, not missed |
| Mass claim surge | Queue depth monitoring | Async job queue + Razorpay bulk API | 2-hour SLA maintained |
| Trigger misfire | Nightly reconciliation vs raw API data | Admin review; model recalibration | No worker impact; internal correction |
| Trigger failure to fire | Watchdog scan every 10 minutes | Admin alert; manual compensation | Resolved within 10 min of detection |
| Premium calculation anomaly | Statistical outlier check on generated premiums | Fallback to rule-based premium | Worker sees corrected premium |

---

## Why GigSafe Will Win

- **Zero-touch claims** — workers never file a claim manually. The system does it for them.
- **Hyper-local triggers** — premiums and triggers are pincode-level, not city-level. A worker in Koramangala gets different pricing than one in Whitefield.
- **Weekly pricing** — perfectly aligned to the gig worker's weekly earning and spending cycle.
- **AI-first from day one** — the ML risk engine is not a Phase 3 addition; it's baked into the core pricing model.
- **Fraud-proof by design** — parametric insurance means claims are triggered by objective third-party data, not worker self-reporting.
- **Syndicate-resistant architecture** — multi-signal Behavioural Consistency Scoring and Isolation Forest ring detection catches coordinated GPS spoofing rings at the population level, not just individually.
- **Worker-first flagging** — honest workers who get flagged face a maximum 6-hour delay, never an automatic rejection. Fairness is a first-class design requirement.
- **Resilient by design** — multi-source API fallbacks, idempotent payouts, async job queues, and stateless backends mean no disruption event is ever missed and no payout is ever lost, even when components fail.
- **Insurance-domain fault detection** — beyond infrastructure health, GigSafe actively monitors for trigger misfires, missed triggers, and calculation drift — faults unique to parametric insurance that generic monitoring tools miss entirely.

---

*Built with ❤️ for India's invisible workforce — the delivery partners keeping our cities running.*
