---
name: system-architect
version: 2.0
description: Generate comprehensive system architecture from any product idea or PRD. Maps 14 layers covering state, journeys, failures, security, resilience, observability, accessibility, and experience. Use before writing any code.
updated: 2026-03-03
---

# System Architect v2

Transform product ideas into comprehensive system architecture before code gets written.

## Core Philosophy

Most apps fail not because features don't work, but because **features don't connect.**

This skill forces systems thinking by mapping **fourteen architecture layers** from any product description. Layers 1–7 were the original framework. Layers 8–14 were added after a RALPH analysis (Systems Thinking → First Principles → Second-Order Thinking) revealed critical blind spots in production.


## Why 14 Layers (The RALPH Analysis)

The original 7-layer framework assumed a connected, single-user, English-speaking, able-bodied user on a fast network accessing a static system.

**Real systems operate in chaos:** offline, multi-device, multi-language, regulated, evolving.

**First Principles reveals:** Every assumption we didn't question became a blind spot.

**Second-Order Thinking reveals:** Missing layers compound.
- No persistence → No offline → No multi-device → Lost users
- No security boundaries → Features accidentally expose data
- No observability → Debugging becomes archaeology
- No accessibility → Legal liability + 15% market exclusion

Adding layers early is 10x cheaper than retrofitting.

### Layer Priority (when time is limited)

**P0 — Build before any code:**
- Persistence Layer (data loss prevention)
- Security Layer (liability prevention)
- Resilience Layer (user trust)

**P1 — Build before launch:**
- Observability Layer (operational excellence)
- Evolution Layer (maintainability)
- Failure Map (support burden)

**P2 — Build for scale:**
- Accessibility Layer (inclusive design)
- Experience Layer (differentiation)
- Cost Model (optimization)


---

## The Fourteen Layers

| # | Layer | Question Answered | Output |
|---|-------|-------------------|--------|
| 1 | System Map | What state connects features? | Features → State → Read/Write relationships |
| 2 | Journey Map | What order must things happen? | Init flow, critical paths, user states |
| 3 | Failure Map | What breaks and what catches it? | Failure modes → Fallbacks → User messaging |
| 4 | Rules Engine | What logic governs behavior? | Triggers → Conditions → Actions |
| 5 | Dependency Map | What external services are needed? | Services → Failure points → Fallbacks |
| 6 | Validation Schema | What gates AI/data quality? | Inputs → Checks → Rejection handling |
| 7 | Cost Model | Where does money/compute go? | Operations → Cost → Optimization opportunities |
| 8 | **Persistence Layer** | How does data survive and sync? | Storage → Sync → Conflict resolution |
| 9 | **Security Layer** | Who can access what? | Auth → Authorization → Encryption → Audit |
| 10 | **Resilience Layer** | What works without network? | Offline → Queue → Sync → Retry |
| 11 | **Observability Layer** | How do we know it's working? | Metrics → Logs → Traces → Alerts |
| 12 | **Accessibility Layer** | Can everyone use it? | Screen readers → Keyboard → Cognitive load |
| 13 | **Evolution Layer** | How do we ship changes? | Migration → Feature flags → Versioning |
| 14 | **Experience Layer** | How does it feel? | Performance → Emotion → Localization |

---

## LAYER 1: System Map

### Purpose
Map how features connect through shared state.

### Process
1. Extract all features from the product description
2. For each feature, identify:
   - What state does it READ to function?
   - What state does it WRITE when used?
3. State that multiple features touch = shared state

### Output Format
```
Feature: [Name] (INIT if first-run required)
├── Reads: [state1, state2, ...]
├── Writes: [state3, state4, ...]
└── Connections: [features that depend on its writes]
```

### Key Questions
- What's the single source of truth for each piece of state?
- Which feature MUST run first?
- What happens if init feature never completes?

---

## LAYER 2: Journey Map

### Purpose
Define the sequence of user actions and system responses.

### Process
1. Identify user states:
   - Uninitialized (first-time user)
   - Active (normal operation)
   - Edge states (expired, banned, incomplete)
2. Map critical paths:
   - First-time setup flow
   - Daily active use flow
   - Recovery flows (re-engagement, error recovery)
3. Define state transitions

### Output Format
```
User State: [Name]
├── Entry Condition: [how user enters this state]
├── Available Actions: [what they can do]
├── Blocked Actions: [what's disabled and why]
└── Exit Conditions: [how they leave this state]
```

### Key Questions
- What does a returning user see?
- What happens if user abandons mid-flow?
- How do we detect which state user is in?

---

## LAYER 3: Failure Map

### Purpose
Enumerate everything that can fail and how we handle it.

### Categories of Failure
1. **Network Failures**: API timeouts, offline, bad responses
2. **User Input Failures**: Invalid data, missing required fields
3. **AI/ML Failures**: Hallucination, wrong format, confidence too low
4. **System Failures**: Storage full, memory exhausted, crash
5. **Business Logic Failures**: Insufficient funds, expired subscription

### Output Format
```
Failure: [Description]
├── Detection: [how we know it happened]
├── Fallback: [what we do instead]
├── User Message: [what user sees]
├── Recovery Path: [how user fixes it]
└── Logging: [what we capture for debugging]
```

### Key Questions
- What's the blast radius of each failure?
- Can failures cascade?
- What's the MTTR (mean time to recovery)?

---

## LAYER 4: Rules Engine

### Purpose
Extract business logic that acts on state independent of user action.

### Categories
1. **Time-based**: "Every 7 days, prompt for progress photo"
2. **Threshold-based**: "If calories exceeded 3 consecutive days, show intervention"
3. **Event-based**: "When workout completed, increment streak"
4. **Composite**: "If inactive 7 days AND has incomplete plan, send re-engagement"

### Output Format
```
Rule: [Name]
├── Trigger: [event/time/threshold]
├── Condition: [state requirements]
├── Action: [what happens]
├── Side Effects: [other state changes]
└── Notification: [user visibility]
```

### Key Questions
- Can rules conflict?
- What's the evaluation order?
- How do we test rules in isolation?

---

## LAYER 5: Dependency Map

### Purpose
Identify all external services and their failure modes.

### Categories
1. **Critical**: App cannot function without (auth, primary DB)
2. **Degraded**: App works but reduced functionality (analytics, recommendations)
3. **Optional**: Nice to have (social features, gamification)

### Output Format
```
Service: [Name]
├── Purpose: [what it provides]
├── Criticality: [Critical/Degraded/Optional]
├── Failure Mode: [timeout/error/rate-limit]
├── Fallback: [cache/default/disable]
├── SLA: [expected availability]
└── Cost: [pricing model]
```

### Key Questions
- What's our dependency depth?
- Do we have single points of failure?
- What's our vendor lock-in risk?

---

## LAYER 6: Validation Schema

### Purpose
Define quality gates for untrusted data entry points.

### Entry Points
1. **User Input**: Forms, uploads, text entry
2. **AI Output**: Generated text, analysis results
3. **External APIs**: Third-party data responses
4. **File Uploads**: Images, documents, imports

### Output Format
```
Entry Point: [Name]
├── Input Type: [expected format]
├── Validations:
│   ├── Required: [fields]
│   ├── Format: [regex/schema]
│   ├── Range: [min/max values]
│   └── Business: [domain-specific rules]
├── Sanitization: [XSS/injection prevention]
├── Rejection UX: [error display]
└── Fallback: [default value if applicable]
```

### Key Questions
- What's the most malicious input possible?
- What happens if validation is bypassed?
- Do we validate on client AND server?

---

## LAYER 7: Cost Model

### Purpose
Identify expensive operations and optimization opportunities.

### Cost Categories
1. **API Calls**: AI tokens, third-party APIs
2. **Storage**: Database, file storage, CDN
3. **Compute**: Serverless invocations, background jobs
4. **Bandwidth**: Data transfer, asset delivery
5. **Third-Party**: Payment processing, email, SMS

### Output Format
```
Operation: [Name]
├── Trigger: [what causes this cost]
├── Frequency: [how often per user]
├── Unit Cost: [$X per invocation]
├── Daily Cost: [estimated at scale]
├── Optimization: [caching/batching/lazy loading]
└── Budget Alert: [threshold for notification]
```

### Key Questions
- What's the cost per active user per day?
- Where are the runaway cost risks?
- What's cacheable?

---

## LAYER 8: Persistence Layer (NEW)

### Purpose
Define how data survives across sessions, devices, and failures.

### Components
1. **Storage Strategy**:
   - Local: IndexedDB, SQLite, AsyncStorage
   - Remote: Cloud DB, API backend
   - Hybrid: Local-first with sync

2. **Sync Architecture**:
   - Real-time (WebSocket)
   - Polling (periodic check)
   - Event-driven (push notifications)
   - Offline-first (queue and sync)

3. **Conflict Resolution**:
   - Last-write-wins
   - Server-wins
   - Client-wins
   - Manual merge (user decides)
   - CRDT (automatic merge)

### Output Format
```
Data Entity: [Name]
├── Storage Location: [local/remote/both]
├── Sync Strategy: [real-time/polling/offline-first]
├── Conflict Resolution: [strategy]
├── TTL: [time-to-live/expiration]
├── Backup: [frequency/location]
└── Deletion: [soft/hard/cascade]
```

### Key Questions
- What survives a clear cache?
- What survives app uninstall?
- How long do we keep data?
- What's the sync conflict scenario?

---

## LAYER 9: Security Layer (NEW)

### Purpose
Map authentication, authorization, encryption, and audit requirements.

### Components
1. **Authentication**:
   - Identity providers (email, OAuth, biometric)
   - Token management (refresh, expiry, revocation)
   - Session handling (timeout, concurrent sessions)

2. **Authorization**:
   - Role-based access control (RBAC)
   - Attribute-based access control (ABAC)
   - Resource ownership (user can only access own data)

3. **Data Protection**:
   - Classification (PII, PHI, sensitive, public)
   - Encryption at rest (what's encrypted)
   - Encryption in transit (TLS requirements)
   - E2E encryption (where applicable)

4. **Audit**:
   - What actions get logged
   - What data is captured
   - Retention period

### Output Format
```
Feature: [Name]
├── Auth Required: [yes/no/optional]
├── Auth Flow: [method]
├── Permissions: [required roles/attributes]
├── Data Classification: [PII/PHI/sensitive/public]
├── Encryption: [at-rest/in-transit/e2e]
├── Audit Events: [actions logged]
└── Compliance: [GDPR/HIPAA/SOC2 requirements]
```

### Key Questions
- Who should NEVER see this data?
- What would a breach expose?
- How do we prove compliance?

---

## LAYER 10: Resilience Layer (NEW)

### Purpose
Design for unreliable networks and distributed failures.

### Components
1. **Offline Capability**:
   - What works without network
   - What degrades gracefully
   - What requires connection

2. **Queue Management**:
   - Operations queued when offline
   - Retry strategy (exponential backoff)
   - Max retries before failure

3. **Network State**:
   - Detection (online/offline/degraded)
   - UI indication (sync status)
   - Transition handling

4. **Optimistic UI**:
   - Show success before confirmation
   - Rollback on failure
   - Pending state indication

### Output Format
```
Feature: [Name]
├── Offline Support: [full/partial/none]
├── Queue Operations: [what gets queued]
├── Retry Strategy: [backoff/max retries]
├── Optimistic UI: [yes/no + rollback]
├── Sync Indicator: [how user knows status]
└── Conflict Handling: [resolution strategy]
```

### Key Questions
- What happens on a subway?
- What if server is down for 2 hours?
- How does user know sync status?

---

## LAYER 11: Observability Layer (NEW)

### Purpose
Instrument the system for operational visibility.

### Components
1. **Metrics**:
   - Business metrics (signups, conversions)
   - Performance metrics (latency, error rates)
   - Infrastructure metrics (CPU, memory)

2. **Logging**:
   - Log levels (error, warn, info, debug)
   - Structured logging (JSON format)
   - Correlation IDs (trace requests)

3. **Tracing**:
   - Distributed tracing across features
   - Span identification
   - Trace visualization

4. **Alerting**:
   - Threshold definitions
   - Alert routing (who gets notified)
   - Escalation paths

### Output Format
```
Feature: [Name]
├── Metrics:
│   ├── [metric_name]: [type] (increment/gauge/histogram)
│   └── Threshold: [alert at X]
├── Logs:
│   ├── Events: [what's logged]
│   └── Level: [error/warn/info]
├── Traces:
│   └── Spans: [what operations create spans]
└── Alerts:
    ├── Condition: [when to fire]
    └── Routing: [who gets notified]
```

### Key Questions
- How will we know something is wrong?
- How do we debug a user's specific issue?
- What's our MTTD (mean time to detect)?

---

## LAYER 12: Accessibility Layer (NEW)

### Purpose
Ensure the product works for all users regardless of ability.

### Components
1. **Visual Accessibility**:
   - Color contrast (WCAG AA/AAA)
   - Font scaling support
   - Dark mode
   - Motion reduction

2. **Screen Reader**:
   - ARIA labels
   - Focus management
   - Announcement timing

3. **Keyboard Navigation**:
   - Tab order
   - Focus traps (modals)
   - Shortcuts

4. **Cognitive Load**:
   - Reading level
   - Error clarity
   - Progressive disclosure

### Output Format
```
Feature: [Name]
├── WCAG Level: [A/AA/AAA]
├── Screen Reader:
│   ├── Labels: [critical elements]
│   └── Announcements: [state changes]
├── Keyboard:
│   ├── Tab Order: [sequence]
│   └── Shortcuts: [available]
├── Cognitive:
│   ├── Reading Level: [grade]
│   └── Decisions Required: [count]
└── Testing: [automated/manual checks]
```

### Key Questions
- Can a blind user complete the critical path?
- Can a keyboard-only user navigate?
- Is the cognitive load appropriate?

---

## LAYER 13: Evolution Layer (NEW)

### Purpose
Design for change, not permanence.

### Components
1. **Schema Migration**:
   - Data model versioning
   - Migration scripts
   - Rollback procedures

2. **Feature Flags**:
   - Rollout percentage
   - Kill switches
   - A/B testing hooks

3. **API Versioning**:
   - Backwards compatibility
   - Deprecation timeline
   - Client version detection

4. **Release Strategy**:
   - Canary deployment
   - Blue-green deployment
   - Progressive rollout

### Output Format
```
Feature: [Name]
├── Schema:
│   ├── Version: [current]
│   └── Migration: [up/down scripts]
├── Feature Flag:
│   ├── Name: [flag_name]
│   └── Default: [on/off]
├── API:
│   ├── Version: [v1/v2/etc]
│   └── Deprecated: [date if applicable]
└── Rollout:
    ├── Strategy: [canary/progressive/big-bang]
    └── Rollback: [procedure]
```

### Key Questions
- What happens when we ship v2?
- Can old and new clients coexist?
- How do we safely remove features?

---

## LAYER 14: Experience Layer (NEW)

### Purpose
Map the emotional and performance experience.

### Components
1. **Performance Budgets**:
   - Load time targets
   - Animation frame budgets (60fps)
   - Memory limits
   - Payload sizes

2. **Emotion Map**:
   - Frustration points
   - Delight moments
   - Trust-building moments
   - Anxiety triggers

3. **Localization**:
   - Supported languages
   - Text expansion handling
   - RTL support
   - Cultural considerations

4. **Microinteractions**:
   - Loading states
   - Success celebrations
   - Error animations
   - Transition designs

### Output Format
```
Feature: [Name]
├── Performance:
│   ├── Load Budget: [Xms]
│   ├── Animation: [60fps required: yes/no]
│   └── Payload: [Xkb max]
├── Emotion:
│   ├── User Feeling: [expected emotion]
│   ├── Frustration Risk: [potential issues]
│   └── Delight Opportunity: [where to exceed expectations]
├── Localization:
│   ├── Languages: [supported]
│   └── RTL: [yes/no]
└── Microinteraction:
    ├── Loading: [spinner/skeleton/progressive]
    └── Success: [celebration type]
```

### Key Questions
- How does the user FEEL at each step?
- What's the performance budget?
- Does this work in other languages?

---

## Process Summary

### Phase 1: Core Architecture (Layers 1-7)
1. Extract features → System Map
2. Define user states → Journey Map
3. Enumerate failures → Failure Map
4. Extract rules → Rules Engine
5. List services → Dependency Map
6. Define gates → Validation Schema
7. Calculate costs → Cost Model

### Phase 2: Infrastructure Architecture (Layers 8-11)
8. Design storage → Persistence Layer
9. Map security → Security Layer
10. Plan resilience → Resilience Layer
11. Instrument → Observability Layer

### Phase 3: Human Architecture (Layers 12-14)
12. Ensure access → Accessibility Layer
13. Plan changes → Evolution Layer
14. Craft experience → Experience Layer

---

## Output Format

Generate a React component that visualizes all fourteen layers as an interactive application. See `references/output-template.md` for the component structure.

The output should:
1. Allow tab navigation between all 14 layers
2. Show relationships visually (hover interactions)
3. Group layers by phase (Core/Infrastructure/Human)
4. Include the raw data as exportable JSON
5. Work as a standalone artifact

---

## References

- `references/output-template.md` - React component template for the visualization
- `references/examples.md` - Example analyses for different product types
- `references/checklists.md` - Per-layer completion checklists


---

## Lesson Log

**2026-03-03 — Sloe Fit (fitness + wellness PWA):**
- Layer 10 (Resilience) is always underestimated for mobile apps — offline handling needs to be designed before state management, not after
- Layer 14 (Experience) should include a "day 30 emotional state" question — why does the user open the app on day 30? If you can't answer it, the architecture is incomplete
- Habit apps live or die on Layer 4 (Rules Engine) — streak logic, intervention triggers, and re-engagement rules need to be explicit before building, not discovered in production
- Layer 9 (Security) for consumer apps: the biggest risk is usually accidental data exposure between users, not external attacks — model your authorization boundaries first

**2026-03-03 — ICON Command Center (retail intelligence dashboard):**
- Layer 11 (Observability) for AI-heavy apps: define what "working correctly" means for AI outputs before launch — hallucination boundaries and confidence thresholds are architecture decisions, not implementation details
- Layer 7 (Cost Model) for Gemini/GPT integrations: token costs at scale are non-obvious — model the cost per DAU before committing to always-on AI features
