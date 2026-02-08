# The Prowler — Technical Architecture

---

**Ben Hutton — Hutton Technologies**
**February 2026**

---

## System Overview

```mermaid
graph TB
    subgraph Clients["What Users See & Touch"]
        direction LR
        MOB["📱 Mobile App<br/>(Flutter)<br/>iPhone + Android"]
        WEB["💻 Web Portal<br/>(Next.js / React)<br/>Browser-Based"]
    end

    subgraph Gateway["API Gateway Layer"]
        LB["⚖️ Load Balancer<br/>(AWS ALB)<br/>Routes traffic · SSL termination"]
    end

    subgraph Backend["Backend Services — FastAPI (Python)"]
        direction TB
        AUTH["🔐 Auth Service<br/>Signup · Login · MFA<br/>Sessions · Tokens"]
        PROFILE["👤 Profile Service<br/>Athletes · Coaches · Brands<br/>Parents · Admin"]
        SEARCH["🔍 Search Service<br/>Filtered Search · AI Search<br/>Ranking · Suggestions"]
        MEDIA["🎬 Media Service<br/>Upload · Transcode · Stream<br/>Thumbnails · CDN"]
        DEAL["💰 Deal Service<br/>Create · Negotiate · Accept<br/>Track · Complete"]
        PAY["💳 Payment Service<br/>Charge · Hold · Release<br/>Refund · Ledger"]
        COMPLY["📋 Compliance Service<br/>CSC Disclosure · NIL Go<br/>1099 · W-9 · Audit"]
        NOTIFY["🔔 Notification Service<br/>Push · Email · SMS<br/>In-App Alerts"]
        AI["🧠 AI Service<br/>Video Analysis · Embeddings<br/>Scouting Assistant"]
    end

    subgraph Data["Data Layer"]
        direction LR
        PG[("🗄️ PostgreSQL 16<br/>+ pgvector<br/>Primary Database")]
        REDIS[("⚡ Redis<br/>Cache · Sessions<br/>Rate Limiting")]
        S3["☁️ AWS S3<br/>Video · Photos<br/>Documents"]
    end

    subgraph External["Third-Party Integrations"]
        direction LR
        STRIPE["💳 Stripe Connect<br/>Payments · Escrow<br/>Payouts"]
        PERSONA["🪪 Persona<br/>KYC Identity<br/>Verification"]
        TWELVE["🎥 Twelve Labs<br/>Video AI<br/>Analysis"]
        NILGO["📋 NIL Go<br/>CSC Compliance<br/>Reporting"]
        SES["📧 AWS SES<br/>Transactional<br/>Email"]
        SNS["📱 AWS SNS<br/>Push<br/>Notifications"]
    end

    subgraph Delivery["Content Delivery"]
        CDN["🌐 CloudFront CDN<br/>Global Edge Network<br/>Fast Media Delivery"]
    end

    MOB --> LB
    WEB --> LB
    LB --> Backend

    AUTH --> PG
    AUTH --> REDIS
    PROFILE --> PG
    SEARCH --> PG
    MEDIA --> S3
    DEAL --> PG
    PAY --> STRIPE
    PAY --> PG
    COMPLY --> NILGO
    COMPLY --> PG
    NOTIFY --> SES
    NOTIFY --> SNS
    AI --> TWELVE
    AI --> PG
    DEAL --> PERSONA

    S3 --> CDN
```

---

## Mobile App Architecture (Flutter)

```mermaid
graph TB
    subgraph App["📱 The Prowler Mobile App"]
        direction TB

        subgraph Screens["Screens — What the User Sees"]
            direction LR
            ONBOARD["Onboarding<br/>& Signup"]
            HOME["Home<br/>Feed"]
            PROF_S["My<br/>Profile"]
            FILM["Film &<br/>Highlights"]
            DEALS_S["My<br/>Deals"]
            SETTINGS["Settings"]
        end

        subgraph State["State Management"]
            direction LR
            AUTH_S["Auth<br/>State"]
            PROFILE_S["Profile<br/>State"]
            MEDIA_S["Media<br/>State"]
            DEAL_STATE["Deal<br/>State"]
            NOTIFY_S["Notification<br/>State"]
        end

        subgraph Services_M["Services Layer"]
            direction LR
            API_CLIENT["API Client<br/>(HTTP + WebSocket)"]
            CACHE_M["Local Cache<br/>(SQLite)"]
            UPLOAD["Upload Manager<br/>(Chunked + Resumable)"]
            PUSH["Push Notification<br/>Handler"]
        end
    end

    subgraph Phone["Device Features"]
        direction LR
        CAMERA["📷 Camera"]
        GALLERY["🖼️ Gallery"]
        BIOMETRIC["🔐 Face ID /<br/>Fingerprint"]
        STORAGE["💾 Local<br/>Storage"]
    end

    Screens --> State
    State --> Services_M
    API_CLIENT --> LB2["⚖️ Load Balancer → Backend"]
    UPLOAD --> S3_2["☁️ Direct to S3"]
    PUSH --> SNS_2["📱 AWS SNS"]
    Services_M --> Phone
```

---

## Web Portal Architecture (Next.js)

```mermaid
graph TB
    subgraph Portal["💻 Web Portal"]
        direction TB

        subgraph Pages["Pages"]
            direction LR
            DASH["Dashboard"]
            SEARCH_P["Athlete<br/>Search"]
            AI_SCOUT["AI Scouting<br/>Assistant"]
            LISTS["Recruit<br/>Lists"]
            BRAND_P["Brand<br/>Portal"]
            DEALS_P["Deal<br/>Management"]
            ADMIN["Admin<br/>Panel"]
        end

        subgraph Components["Reusable Components"]
            direction LR
            CARD["Athlete<br/>Cards"]
            PLAYER["Video<br/>Player"]
            FILTERS["Search<br/>Filters"]
            CHARTS["Analytics<br/>Charts"]
            CHAT_C["AI Chat<br/>Interface"]
            TABLE["Data<br/>Tables"]
        end

        subgraph ClientLogic["Client-Side Logic"]
            direction LR
            AUTH_C["Auth<br/>Handler"]
            FETCH["API<br/>Client"]
            WS["WebSocket<br/>(Real-Time)"]
            STORE["State<br/>Store"]
        end
    end

    Pages --> Components
    Components --> ClientLogic
    FETCH --> LB3["⚖️ Load Balancer → Backend"]
    WS --> LB3
```

---

## Database Schema

```mermaid
erDiagram
    USERS {
        uuid id PK
        string email UK
        string password_hash
        string role
        boolean mfa_enabled
        timestamp created_at
    }

    ATHLETE_PROFILES {
        uuid id PK
        uuid user_id FK
        string school
        string sport
        string position
        int grad_year
        float gpa
        string state
        string city
        jsonb stats
        vector embedding
    }

    COACH_PROFILES {
        uuid id PK
        uuid user_id FK
        string school
        string division
        string sport
        string subscription_tier
    }

    BRAND_PROFILES {
        uuid id PK
        uuid user_id FK
        string company_name
        string industry
        boolean kyc_verified
    }

    PARENT_PROFILES {
        uuid id PK
        uuid user_id FK
        uuid linked_athlete FK
    }

    MEDIA {
        uuid id PK
        uuid athlete_id FK
        string type
        string s3_key
        string cdn_url
        string status
        int duration_seconds
        jsonb ai_tags
        vector embedding
    }

    HIGHLIGHTS {
        uuid id PK
        uuid media_id FK
        int start_ms
        int end_ms
        string play_type
        float confidence
        string thumbnail_url
    }

    NIL_DEALS {
        uuid id PK
        uuid athlete_id FK
        uuid brand_id FK
        decimal amount
        string status
        string deliverables
        date deadline
        timestamp accepted_at
    }

    CONTRACTS {
        uuid id PK
        uuid deal_id FK
        string document_url
        boolean athlete_signed
        boolean brand_signed
        timestamp signed_at
    }

    ESCROW {
        uuid id PK
        uuid deal_id FK
        string stripe_payment_intent
        decimal amount
        string status
        timestamp captured_at
        timestamp released_at
    }

    CSC_FILINGS {
        uuid id PK
        uuid deal_id FK
        string nilgo_reference
        string disclosure_document
        string status
        timestamp submitted_at
        date deadline
    }

    TAX_RECORDS {
        uuid id PK
        uuid athlete_id FK
        int tax_year
        decimal total_earnings
        string w9_document
        string form_1099_url
        boolean issued
    }

    USERS ||--o| ATHLETE_PROFILES : "has"
    USERS ||--o| COACH_PROFILES : "has"
    USERS ||--o| BRAND_PROFILES : "has"
    USERS ||--o| PARENT_PROFILES : "has"
    PARENT_PROFILES ||--|| ATHLETE_PROFILES : "linked to"

    ATHLETE_PROFILES ||--o{ MEDIA : "uploads"
    MEDIA ||--o{ HIGHLIGHTS : "generates"

    ATHLETE_PROFILES ||--o{ NIL_DEALS : "earns from"
    BRAND_PROFILES ||--o{ NIL_DEALS : "sponsors"

    NIL_DEALS ||--|| CONTRACTS : "governed by"
    NIL_DEALS ||--|| ESCROW : "funded through"
    NIL_DEALS ||--o| CSC_FILINGS : "reported via"

    ATHLETE_PROFILES ||--o{ TAX_RECORDS : "files"
```

---

## Authentication & Security Flow

```mermaid
sequenceDiagram
    participant User
    participant App as Mobile App / Web Portal
    participant API as Backend API
    participant DB as Database
    participant Redis as Redis Cache
    participant Persona as Persona (ID Check)

    Note over User,Persona: Account Creation
    User->>App: Sign up (email + password)
    App->>API: POST /auth/register
    API->>DB: Create user record
    API->>User: Verification email sent

    Note over User,Persona: Login
    User->>App: Enter email + password
    App->>API: POST /auth/login
    API->>DB: Verify credentials
    API->>Redis: Store session token (24hr expiry)
    API->>App: Return JWT token + refresh token

    Note over User,Persona: Identity Verification (for marketplace)
    User->>App: Start KYC verification
    App->>API: POST /auth/kyc/start
    API->>Persona: Create verification session
    Persona->>User: Government ID upload + live selfie
    Persona->>API: Verification result (pass/fail)
    API->>DB: Update KYC status

    Note over User,Persona: Multi-Factor Auth (for payments)
    User->>App: Initiate payment action
    App->>API: Request MFA challenge
    API->>User: Send 6-digit code (SMS or email)
    User->>App: Enter code
    App->>API: Verify code
    API->>App: Payment action authorized ✅
```

---

## Video Processing Pipeline

```mermaid
flowchart TB
    subgraph Upload["1 · Upload"]
        USER_UP["Athlete selects<br/>video from phone"]
        CHUNK["App splits video<br/>into chunks<br/>(resumable upload)"]
        S3_UP["Chunks uploaded<br/>directly to S3"]
        MERGE["Server merges<br/>chunks into<br/>final file"]
    end

    subgraph Process["2 · Process"]
        THUMB["Generate<br/>thumbnail<br/>images"]
        TRANSCODE["Transcode to<br/>multiple qualities<br/>(480p · 720p · 1080p)"]
        HLS["Create streaming<br/>format<br/>(HLS segments)"]
    end

    subgraph AIAnalysis["3 · AI Analysis"]
        INDEX["Send to<br/>Twelve Labs<br/>for indexing"]
        DETECT["AI detects<br/>key plays:<br/>TD · INT · Tackle<br/>Catch · Block"]
        CLIP["Auto-cut<br/>highlight<br/>clips"]
        EMBED["Generate<br/>vector embeddings<br/>(for AI search)"]
    end

    subgraph Output["4 · Available"]
        STREAM["🎬 Streamable<br/>video on CDN"]
        HIGHLIGHTS_O["📱 Auto-generated<br/>highlight reel"]
        SEARCHABLE["🔍 Film searchable<br/>by coaches via AI"]
    end

    USER_UP --> CHUNK --> S3_UP --> MERGE
    MERGE --> THUMB & TRANSCODE
    TRANSCODE --> HLS
    MERGE --> INDEX --> DETECT --> CLIP
    INDEX --> EMBED

    HLS --> STREAM
    CLIP --> HIGHLIGHTS_O
    EMBED --> SEARCHABLE
```

---

## AI Search Pipeline

```mermaid
flowchart TB
    subgraph Input["Coach's Search Query"]
        QUERY["'Find me a 6'2 safety in Georgia<br/>with good ball-hawking instincts<br/>and a 3.5+ GPA'"]
    end

    subgraph Parse["Query Parser"]
        HARD["Extract hard filters:<br/>Position = Safety<br/>Height ≥ 6'2<br/>State = Georgia<br/>GPA ≥ 3.5"]
        SOFT["Extract soft concepts:<br/>'ball-hawking instincts'<br/>(no database column for this)"]
    end

    subgraph Structured["Structured Search (PostgreSQL)"]
        SQL["SELECT athletes WHERE<br/>position = 'S'<br/>AND height >= 74<br/>AND state = 'GA'<br/>AND gpa >= 3.5"]
        RESULTS_S["47 athletes match<br/>hard criteria"]
    end

    subgraph Vector["AI Search (pgvector)"]
        VECTORIZE["Convert 'ball-hawking instincts'<br/>into a vector embedding"]
        SIMILAR["Find athletes whose<br/>scouting reports + film tags<br/>are semantically similar"]
        RESULTS_V["Scored by<br/>relevance"]
    end

    subgraph Combine["Combine & Rank"]
        MERGE_R["Merge both result sets"]
        RANK["Rank by:<br/>Hard-filter match score<br/>+ AI relevance score<br/>+ Profile completeness"]
        FINAL["📋 Top 15 Athletes<br/>Ranked with Evidence"]
    end

    QUERY --> HARD & SOFT
    HARD --> SQL --> RESULTS_S
    SOFT --> VECTORIZE --> SIMILAR --> RESULTS_V
    RESULTS_S --> MERGE_R
    RESULTS_V --> MERGE_R
    MERGE_R --> RANK --> FINAL
```

---

## NIL Deal Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Draft: Brand creates deal
    Draft --> Proposed: Brand submits offer
    Proposed --> Negotiation: Athlete requests changes
    Negotiation --> Proposed: Brand revises
    Proposed --> Accepted: Athlete accepts terms
    Proposed --> Declined: Athlete declines

    Accepted --> KYC_Check: Verify identities
    KYC_Check --> KYC_Failed: Verification fails
    KYC_Failed --> KYC_Check: Retry verification
    KYC_Check --> Contract_Signing: Both verified ✅

    Contract_Signing --> Funded: Both parties sign, brand's payment authorized
    Funded --> Escrow_Held: Funds captured and held
    Escrow_Held --> In_Progress: Athlete begins work

    state compliance_fork <<fork>>
    In_Progress --> compliance_fork

    compliance_fork --> CSC_Filed: Auto-submit disclosure (if ≥$600)
    compliance_fork --> Work_Submitted: Athlete submits proof

    state compliance_join <<join>>
    CSC_Filed --> compliance_join
    Work_Submitted --> compliance_join

    compliance_join --> Under_Review: Platform reviews deliverables
    Under_Review --> Revision_Requested: Work needs changes
    Revision_Requested --> Work_Submitted: Athlete resubmits

    Under_Review --> Approved: Deliverables verified ✅
    Approved --> Payment_Released: Escrow released to athlete
    Payment_Released --> Complete: 1099 queued if ≥$600 ✅

    Declined --> [*]
    Complete --> [*]
```

---

## Payment & Escrow Flow

```mermaid
sequenceDiagram
    participant Brand
    participant API as The Prowler API
    participant Stripe as Stripe Connect
    participant Escrow as Escrow Account
    participant Athlete
    participant Comply as Compliance Service

    Note over Brand,Comply: Deal Funding
    Brand->>API: Accept deal terms
    API->>Stripe: Create PaymentIntent ($5,000)
    Stripe->>Brand: Charge brand's payment method
    Stripe->>Escrow: Hold funds ($5,000)
    API->>Brand: Payment confirmed ✅

    Note over Brand,Comply: Work Period
    Athlete->>API: Submit deliverable proof
    API->>API: Review deliverables

    Note over Brand,Comply: Payment Release
    API->>Stripe: Capture payment
    Stripe->>Escrow: Release from hold

    Note over Brand,Comply: Fee Calculation
    API->>API: Calculate fees
    Note right of API: $5,000.00 - Brand paid<br/>-$145.30 - Stripe processing<br/>-$12.75 - Payout fee<br/>-$500.00 - Platform fee (10%)<br/>=$4,341.95 - Athlete receives

    Stripe->>Athlete: Deposit $4,341.95
    API->>Comply: Trigger compliance workflow

    Note over Brand,Comply: Compliance (if deal ≥ $600)
    Comply->>Comply: Generate CSC disclosure
    Comply->>Comply: Submit to NIL Go
    Comply->>Comply: Queue 1099 for year-end
```

---

## Infrastructure & Deployment

```mermaid
graph TB
    subgraph Development["🖥️ Development Environment"]
        DEV_MACHINE["Developer Machine"]
        DOCKER["Docker Compose<br/>API · DB · Redis · S3 mock"]
        TESTS["Automated Tests<br/>(pytest)"]
    end

    subgraph CICD["🔄 CI/CD Pipeline (GitHub Actions)"]
        PUSH["Code pushed<br/>to GitHub"]
        LINT["Lint &<br/>Type Check"]
        TEST["Run<br/>Test Suite"]
        BUILD["Build Docker<br/>Image"]
        SCAN["Security<br/>Scan"]
        ECR["Push to AWS<br/>Container Registry"]
    end

    subgraph Staging["🧪 Staging Environment"]
        S_ECS["ECS Fargate<br/>(1 instance)"]
        S_RDS["RDS PostgreSQL<br/>(test data)"]
        S_REDIS["ElastiCache Redis"]
    end

    subgraph Production["🚀 Production Environment (AWS)"]

        subgraph Network["Network Layer"]
            WAF["AWS WAF<br/>(Firewall)"]
            ALB_P["Application<br/>Load Balancer"]
        end

        subgraph Compute["Compute Layer"]
            ECS_1["ECS Task 1"]
            ECS_2["ECS Task 2"]
            ECS_N["ECS Task N<br/>(auto-scale 2–8)"]
        end

        subgraph Storage["Storage Layer"]
            RDS_P[("RDS PostgreSQL 16<br/>Multi-AZ<br/>Auto-backup")]
            REDIS_P[("ElastiCache Redis<br/>2 nodes")]
            S3_P["S3 Buckets<br/>Media · Documents<br/>Backups"]
        end

        subgraph Edge["Edge & Delivery"]
            CF["CloudFront CDN<br/>(200+ global locations)"]
        end

        subgraph Monitoring["Monitoring"]
            CW["CloudWatch<br/>Metrics · Logs"]
            SENTRY["Sentry<br/>Error Tracking"]
            ALERTS["SNS Alerts<br/>PagerDuty"]
        end
    end

    DEV_MACHINE --> DOCKER
    DOCKER --> TESTS
    TESTS --> PUSH

    PUSH --> LINT --> TEST --> BUILD --> SCAN --> ECR

    ECR --> S_ECS
    ECR --> Compute

    WAF --> ALB_P --> Compute
    Compute --> RDS_P
    Compute --> REDIS_P
    Compute --> S3_P
    S3_P --> CF

    Compute --> Monitoring
```

---

## Auto-Scaling Rules

```mermaid
graph LR
    subgraph Triggers["What Triggers Scaling"]
        CPU["CPU > 70%<br/>for 3 minutes"]
        MEM["Memory > 80%<br/>for 3 minutes"]
        REQ["Request count ><br/>1,000/min"]
    end

    subgraph Scale["Scaling Actions"]
        UP["Scale UP<br/>Add 1 server<br/>(up to 8 max)"]
        DOWN["Scale DOWN<br/>Remove 1 server<br/>(down to 2 min)<br/>after 10 min cool-down"]
    end

    subgraph Cost["Monthly Compute Cost"]
        MIN["2 servers<br/>~$58/mo"]
        MID["4 servers<br/>~$116/mo"]
        MAX["8 servers<br/>~$232/mo"]
    end

    CPU --> UP
    MEM --> UP
    REQ --> UP
    UP --> MID --> MAX
    MAX -.->|"Traffic drops"| DOWN
    DOWN --> MID --> MIN
```

---

## Full Request Lifecycle

*What happens when a coach searches for an athlete — every system touched in order:*

```mermaid
sequenceDiagram
    participant Coach
    participant Browser as Web Portal
    participant CDN as CloudFront CDN
    participant LB as Load Balancer
    participant API as FastAPI Backend
    participant Cache as Redis Cache
    participant DB as PostgreSQL
    participant AI as pgvector (AI Search)

    Coach->>Browser: Type search query
    Browser->>LB: GET /api/v1/athletes/search?...
    LB->>API: Route to available server

    API->>Cache: Check cache for this query
    alt Cache hit
        Cache->>API: Return cached results
    else Cache miss
        API->>DB: Run structured filters
        DB->>API: Return matching athletes
        API->>AI: Run vector similarity search
        AI->>API: Return AI-ranked matches
        API->>API: Merge & rank results
        API->>Cache: Store results (5 min TTL)
    end

    API->>LB: Return JSON response
    LB->>Browser: Display athlete cards

    Coach->>Browser: Click athlete card
    Browser->>LB: GET /api/v1/athletes/{id}
    LB->>API: Fetch full profile
    API->>DB: Get profile + stats + deals
    API->>Browser: Return profile data

    Coach->>Browser: Play highlight reel
    Browser->>CDN: Request video stream
    CDN->>Browser: Stream from nearest edge server
```

---

## Technology Stack Summary

```mermaid
mindmap
    root["The Prowler<br/>Tech Stack"]
        Frontend
            Flutter
                iOS App
                Android App
            Next.js
                Coach Portal
                Brand Portal
                Admin Panel
        Backend
            FastAPI — Python
                REST API
                WebSocket
                Background Jobs
        Database
            PostgreSQL 16
                Profiles
                Deals
                Compliance
            pgvector
                AI Embeddings
                Similarity Search
            Redis
                Session Cache
                Query Cache
                Rate Limiting
        Cloud — AWS
            ECS Fargate — Compute
            RDS — Database
            S3 — Storage
            CloudFront — CDN
            SES — Email
            SNS — Push
            Secrets Manager
            WAF — Firewall
        Integrations
            Stripe Connect
                Payments
                Escrow
                Payouts
            Persona
                KYC
                ID Verification
            Twelve Labs
                Video AI
                Highlights
            NIL Go
                CSC Filings
                Disclosures
```

---

*Ben Hutton — Hutton Technologies — February 2026*
