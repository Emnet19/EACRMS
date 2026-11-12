COLLEGE OF ENGINEERING
DEPARTMENT OF SOFTWARE ENGINEERING

Internship Report
Internship at Information Network Security Administration (INSA) in Collaboration with the Ethiopian Athletics Federation (EAF)

Design, Implementation, and Full-Stack CI/CD Deployment of the Ethiopian Athletics Federation Digital Hub (EOSCRMS)

Prepared by:
Student Name: [Full Name]
Student ID: [ID Number]
Industry Supervisor: [Name]
University Supervisor: [Name]

Addis Ababa, Ethiopia
September, 2026

APPROVAL PAGE

This report has been prepared and submitted by [Student Name] based on the software engineering internship completed at Information Network Security Administration (INSA) in collaboration with the Ethiopian Athletics Federation (EAF).

Industry Supervisor
Name: __________________________
Reference (Phone, Email, or other): ____________________________________________
Signature: ______________________
Date: ___________________________

University Supervisor
Name: __________________________
Signature: ______________________
Date: ___________________________

DECLARATION

I, [Student Name], declare that this internship final report is based on the internship experience I completed at Information Network Security Administration (INSA) working on the Ethiopian Athletics Federation digital systems.
I confirm that the information presented in this report is accurate to the best of my knowledge and that all sources used have been properly acknowledged.

Student Name: ______________________
Signature: __________________________
Date: _______________________________

ABSTRACT

This report documents the design, development, and production containerization of the Ethiopian Athletics Federation (EAF) Digital Hub application, known as EOSCRMS. Completed during a twelve-week internship at the Information Network Security Administration (INSA) in Addis Ababa, the project addresses chronic bottlenecks in national athletic management. Historical administration relied on paper forms, manual document submissions, and fragmented regional club archives. This practice generated identity disputes, licensing delays, and administrative overhead during national track meets.

The engineering solution encompasses a modern web client and a complete DevOps delivery pipeline. The initial four weeks established a full-stack CI/CD infrastructure based on the institutional syllabus. The full application was containerized across independent frontend, backend, and PostgreSQL database tiers using Docker Compose, internal bridge networks, and persistent named volumes. Production artifacts were managed on Docker Hub using deterministic Git Commit SHA tags. An automated Continuous Integration pipeline was deployed using GitHub Actions to enforce linting, unit testing, and automated image building on pull requests. Continuous Deployment webhooks triggered zero-downtime rollouts to cloud compute environments on Render and Vercel. Production traffic routing was hardened using an Nginx reverse proxy load balancer distributing requests across backend replicas, monitored by Prometheus metrics scrapers and Grafana operational dashboards.

During the subsequent eight weeks, application engineering delivered the client interface on React 18.3.1, TypeScript, and Vite, synchronized through Redux Toolkit and RTK Query. Key deliverables include direct integration with the Fayda National Digital ID system under Proclamation No. 1284/2023, biometric hash checks, digital athlete licensing with dynamic 120-second QR codes, and GPS geofenced call room check-ins for race venues. A club administration suite handles roster tracking, inter-club transfers with federation clearance locks, and an automated seeding engine that distributes athletes into heats and lanes based on Season Best and Personal Best marks. The user interface supports four Ethiopian languages: Amharic, Afaan Oromo, Tigrinya, and English. Testing confirmed immediate reductions in registration turnaround times and accurate lane assignments. This report covers system requirements, engineering activities, technical skills gained, academic course applications at AASTU, and recommendations for future engineering iterations.

TABLE OF CONTENTS

1. Chapter 1: Introduction
   1.1. Background of the Internship
   1.2. Purpose of the Internship
   1.3. Objectives of the Internship
        1.3.1. General Objective
        1.3.2. Specific Objectives
   1.4. Scope of the Internship
   1.5. Host Organization
   1.6. Internship Department or Team

2. Chapter 2: Internship Activities and Project Work
   2.1. Overview
   2.2. Software Engineering and DevOps Activities
   2.3. Weekly Internship Activities (Weeks 1 to 12)
   2.4. Major Projects: EAF Digital Hub (EOSCRMS)
        2.4.1. Project Background
        2.4.2. Project Objective
        2.4.3. System Requirements
        2.4.4. System Design and Multi-Container Architecture
        2.4.5. Technologies and Tools Used
        2.4.6. Implementation Phases, CI/CD Pipeline and Testing
        2.4.7. Production Hardening: Nginx Load Balancing and Monitoring
        2.4.8. Project Results

3. Chapter 3: Skills, Challenges, and Lessons Learned
   3.1. Technical Skills Gained
   3.2. Software Engineering and DevOps Practices
   3.3. Professional Skills Gained
   3.4. Application of Academic Knowledge
   3.5. Challenges Encountered
        3.5.1. Technical Challenges
        3.5.2. Workplace Challenges
   3.6. Solutions Applied
   3.7. Lessons Learned

4. Chapter 4: Conclusion and Recommendations
   4.1. Conclusion
   4.2. Recommendations to the Host Organization
   4.3. Recommendations to the Department of Software Engineering
   4.4. Recommendations to Future Students

5. Annex
   Annex A: System Navigation and Core Routes
   Annex B: End-to-End CI/CD Pipeline Workflow

Chapter 1: Introduction

1.1. Background of the Internship

Software engineering education requires a transition from isolated classroom exercises to production environments. At Addis Ababa Science and Technology University (AASTU), the Department of Software Engineering structures its curriculum to build strong foundations in algorithms, database design, software architecture, and system modeling. Classroom projects run in predictable settings. Real institutional systems operate with imperfect data, shifting user demands, distributed networks, and stringent compliance constraints.

Field internships give students direct exposure to production engineering teams. During this period, students work with established code repositories, follow team code review processes, and deploy functional software artifacts. Working at the Information Network Security Administration (INSA) on national sports infrastructure provided this bridge. The experience demonstrated how software engineering principles guide the construction of public digital registries that serve athletes, sports clubs, and government regulators across Ethiopia.

1.2. Purpose of the Internship

The internship fulfilled several educational and professional goals:

First, it offered practical experience in full-stack web engineering using modern frontend libraries, state containers, and asynchronous communication patterns. Second, it delivered hands-on training in contemporary DevOps workflows, covering multi-container Dockerization, automated CI pipeline gating, cloud deployment, and system observability. Third, it allowed the application of theoretical concepts from AASTU courses such as Object-Oriented Programming, Database Systems, Computer Networks, and Operating Systems to real business problems. Fourth, it exposed me to team workflows, including agile sprint planning, daily standup reviews, branch protection rules, and code quality audits. Finally, the internship built essential workplace skills in technical communication, project estimation, and collaborating with domain specialists who possess little software engineering background.

1.3. Objectives of the Internship

1.3.1. General Objective

The general objective was to design, implement, containerize, and deploy a secure web client portal and automated delivery pipeline for the Ethiopian Athletics Federation that digitizes athlete registration, identity verification, licensing, and competition meet management.

1.3.2. Specific Objectives

To accomplish this general objective, the following specific objectives were completed:

1. Construct a multi-container local runtime with Docker Compose, isolating frontend, backend, and database tiers via internal bridge networks and named persistent volumes during initial onboarding.
2. Establish an automated Continuous Integration pipeline using GitHub Actions to enforce linting, unit testing, and Docker Hub image publication using Git Commit SHA tags.
3. Deploy continuous delivery webhooks connecting repository merges to zero-downtime cloud compute environments on Render and Vercel.
4. Configure an Nginx reverse proxy load balancer distributing traffic across parallel backend replicas, coupled with Prometheus metrics scrapers and Grafana analytical dashboards.
5. Integrate the client with the Fayda National Digital ID verification system under Ethiopian Proclamation No. 1284/2023 to validate athlete identities and prevent age falsification.
6. Build an athlete licensing module that generates tamper-evident digital credentials featuring dynamic 120-second QR codes for competition call room verification.
7. Develop a club management dashboard for roster administration, member registration, and official transfer contract processing with federation clearance locks.
8. Implement an automated race seeding engine that groups registered runners into heats and allocates track lanes using official Season Best and Personal Best metrics.
9. Create a venue check-in interface using browser geolocation APIs to enforce 500-meter stadium geofencing on race days.
10. Provide multilingual localization across four Ethiopian languages: Amharic, Afaan Oromo, Tigrinya, and English.

1.4. Scope of the Internship

The internship covered both client application development and the supporting full-stack CI/CD infrastructure for the Ethiopian Athletics Federation Registry and Competition Management System (EOSCRMS).

On the infrastructure tier, responsibilities followed the structured full-stack CI/CD syllabus during the opening weeks of the internship. I containerized the independent frontend, backend, and PostgreSQL database tiers using Dockerfiles and Docker Compose. I established custom bridge networks and named volumes to ensure transaction persistence. I configured authenticated Docker Hub image publishing pipelines driven by Git Commit SHAs, created automated GitHub Actions workflows with secrets injection and layer caching, and set up cloud deployment hooks. The infrastructure scope concluded with engineering an Nginx ingress reverse proxy distributing load across two backend replicas, monitored by Prometheus and Grafana dashboards.

On the client tier, responsibilities included authoring modular React components in TypeScript, configuring global state slices in Redux Toolkit, and setting up asynchronous caching hooks using RTK Query. I built verification flows for Fayda identity numbers, secondary KYC file uploads, club member rosters, dynamic transfer records, and automated race seeding engines. The scope also included building a mock API service layer to simulate backend endpoints during development, designing responsive interfaces with Tailwind CSS, and implementing four-language internationalization.

1.5. Host Organization

The internship took place at the Information Network Security Administration (INSA) in Addis Ababa, Ethiopia, operating in collaboration with the Ethiopian Athletics Federation (EAF).

INSA was established in 2006 by the Federal Government of Ethiopia to protect the national information infrastructure, develop secure computational systems, and spearhead national digital transformation initiatives. INSA delivers software solutions, cybersecurity audits, cryptography research, and secure data services for key federal ministries and public institutions.

The Ethiopian Athletics Federation, founded in 1964 and affiliated with World Athletics and the African Athletics Confederation, governs track and field, marathon, and cross-country sports throughout Ethiopia. EAF oversees prominent clubs such as Defense Athletics Club, Commercial Bank of Ethiopia AC, Oromia Police SC, and Sidama Coffee AC. The joint project between INSA and EAF aimed to modernize Ethiopian athletic governance by replacing manual administration with a secure digital platform.

1.6. Internship Department or Team

I was assigned to the Software Development and Systems Integration Division within INSA.

The division builds software systems for federal partner organizations. It employs software architects, frontend engineers, backend engineers, DevOps specialists, and quality assurance analysts. The team works in two-week agile sprints. Daily activities begin with fifteen-minute standup meetings to review progress, track blockers, and assign Jira tickets. My role was Frontend and Full-Stack Engineering Intern. I reported directly to the team lead and industry supervisor, who reviewed my pull requests, evaluated component architecture, and approved integration deliverables.

Chapter 2: Internship Activities and Project Work

2.1. Overview

My work encompassed frontend engineering, client state architecture, API integration, and full-stack DevOps automation. Over twelve weeks, I implemented sixteen dedicated user interface components, configured client-side data stores, and established continuous integration and deployment pipelines. The initial four weeks concentrated on container orchestration, cloud registries, automated CI quality gating, and Nginx load balancing. The subsequent eight weeks focused on delivering core athletic management modules, Fayda identity verification, race seeding algorithms, and multilingual interfaces.

2.2. Software Engineering and DevOps Activities

During the internship, I carried out several core software engineering and DevOps responsibilities:

1. Multi-Container Orchestration: Wrote Dockerfiles and docker-compose.yml specifications to isolate frontend, backend, and PostgreSQL tiers. Built internal bridge networks for inter-service communication and attached named storage volumes to guarantee database persistence.
2. Registry Management and Image Tagging Topologies: Established authenticated Docker Hub pipelines using access tokens. Employed deterministic Git Commit SHA tags to ensure repeatable container builds and traceable release histories.
3. Automated CI Pipeline Development: Authored GitHub Actions workflow files that enforce automated linting and unit testing on pull requests, followed by authenticated image compilation and promotion to Docker Hub.
4. Continuous Deployment Execution: Linked repository merge events to cloud platforms through deploy webhooks on Render and Vercel, enforcing zero-downtime traffic switching and strict separation of environment variables from source control.
5. Ingress Routing, Load Balancing, and Observability: Configured Nginx as an ingress reverse proxy to balance incoming HTTP requests across two backend container replicas. Set up Prometheus time-series metric collection and built Grafana operational tracking dashboards.
6. Requirements Analysis and Domain Modeling: Studied World Athletics technical rules and EAF competition regulations. Mapped out business logic for age categorization (Senior, Under-20, Under-18, Under-16), club transfer windows, and track heat lane seeding.
7. Component Architecture Design: Created modular components using React 18.3.1 and Tailwind CSS. Split complex forms into manageable sub-views such as FaydaVerification, RosterManagement, SeedingGenerator, and GeofenceCheckin.
8. State Management Engineering: Built centralized Redux slices for authentication, athlete records, and club details. Wired up RTK Query endpoints with automatic bearer token injection and silent token refresh handlers.
9. Security and Identity Integration: Integrated client-side hash generation for Fayda Identity Numbers (FIN) under Proclamation No. 1284/2023. Added dynamic QR code generation that cycles tokens every two minutes to prevent credential sharing.
10. Location Services Engineering: Connected the HTML5 Geolocation API with haversine distance formulas to calculate proximity to competition grounds like Addis Ababa National Stadium and Jan Meda.
11. Internationalization: Implemented language dictionary tables in Amharic, Afaan Oromo, Tigrinya, and English, maintaining consistent terminology for athletic events and administrative statuses.

2.3. Weekly Internship Activities (Weeks 1 to 12)

The following table details the major engineering activities and deliverables completed across all twelve weeks of the internship:

| Week | Major Activities | Main Output |
|---|---|---|
| Week 1 | Orientation, workstation configuration, repository setup, Git mastery, and local multi-container Dockerization. Containerized frontend, backend, and PostgreSQL tiers in docker-compose.yml with internal bridge networks and named volumes. | Multi-container local environment running with sudo docker compose up. Database persistence verified across container tear-downs. |
| Week 2 | Docker Hub registry management, image tagging topologies, and remote cloud pull validation. Implemented Git Commit SHA image tagging rules (eaf-client:${GITHUB_SHA}) and pushed images to authenticated Docker Hub repositories. | Automated container publishing verified on remote cloud compute instance without local source files. |
| Week 3 | Continuous Integration and Continuous Deployment pipeline construction. Authored GitHub Actions workflows enforcing automated ESLint and unit testing on pull requests, with layer caching and deploy webhooks to Render and Vercel. | Automated CI quality gating and zero-downtime cloud continuous deployment pipeline operational. |
| Week 4 | Production ingress routing, Nginx load balancing, and observability stack setup. Configured Nginx upstream reverse proxy balancing traffic across two backend container replicas, coupled with Prometheus scraping and Grafana tracking dashboards. | Ingress load balancer and Grafana telemetry dashboards operating with live CPU and latency metric graphs. |
| Week 5 | Base frontend architecture, responsive layout tokens, and navigation components. Established global Redux Toolkit store slices, base card components, responsive navbar, and dark mode toggles with Tailwind CSS. | Base client layout, design tokens, and state architecture committed to git. |
| Week 6 | Fayda National Digital ID verification module integration. Implemented FIN input validation under Proclamation No. 1284/2023, cryptographic hash simulation, and secondary KYC document upload dialogs for youth divisions. | FaydaVerification.tsx component operational and linked to athlete profile state. |
| Week 7 | Club administration dashboard and roster management. Built club metrics overview, member table with license filtering, and new athlete registration forms with local persistence fallbacks. | RosterManagement.tsx and RegisterMember.tsx modules completed with live filtering. |
| Week 8 | Federation transfer registry and transfer window lockdown mechanism. Constructed transfer agreement modal, transfer fee inputs, contract hash tracking, and federation clearance approval state machines. | TransferRegistry.tsx module operational with contract status badges. |
| Week 9 | Competition meet registration and automated track seeding engine. Engineered heat and lane allocation algorithms using official Season Best (SB) and Personal Best (PB) metrics for championship events. | SeedingGenerator.tsx and MeetRegistration.tsx modules delivering automated lane draws. |
| Week 10 | GPS venue geofencing and dynamic call room QR code credentials. Integrated HTML5 Geolocation API with haversine distance checks for Addis Ababa Stadium, and implemented 120-second dynamic QR token rotation. | GeofenceCheckin.tsx and LicenseQrCode.tsx modules active. |
| Week 11 | Multi-language internationalization and Ge'ez typography baseline optimization. Built translation dictionaries across Amharic, Afaan Oromo, Tigrinya, and English, with custom CSS line-height rules for Ethiopic fonts. | Four-language localization active across all portal screens without layout shift. |
| Week 12 | End-to-end integration testing, load resilience validation, documentation, and production handover. Ran automated test suites across the GitHub Actions pipeline, simulated traffic on Nginx replicas, and finalized system technical manuals. | Production-ready software artifact, verified CI/CD delivery pipeline, and final internship documentation delivered. |

2.4. Major Projects: EAF Digital Hub (EOSCRMS)

2.4.1. Project Background

The Ethiopian Athletics Federation oversees thousands of competitive runners distributed across regional clubs, police sports societies, military sports divisions, and youth academies. Historically, athlete registration relied on paper forms submitted in person at the federation headquarters in Addis Ababa.

This paper-based workflow caused severe administrative problems. Verification officers struggled to detect forged birth certificates and duplicate registrations. Athletes occasionally signed contracts with two clubs simultaneously. When major track championships approached, federation clerks spent days sorting paper entry slips to build competition starter lists. Seeding errors occurred frequently. Club managers waited weeks to receive official athlete competition licenses. The EOSCRMS portal was initiated to replace these paper methods with a reliable, browser-accessible digital management hub backed by automated deployment pipelines.

2.4.2. Project Objective

The primary objective was to deliver a production-ready client application and operational deployment infrastructure that allows athletes to verify their digital identity and access event credentials, enables club managers to maintain rosters and submit meet entries, and provides federation marshals with verified starter lists, call room validation tools, and reliable server uptime.

2.4.3. System Requirements

Functional Requirements:
- Athlete Identity Verification: The system must validate the athlete Fayda Identification Number (FIN), display verified biographic data from government records, and accept secondary KYC documents for youth age divisions.
- Digital License Management: The system must display active federation licenses, track expiration dates, and render a dynamic QR credential that updates its token every 120 seconds.
- GPS Geofenced Call Room Check-In: The system must verify the athlete physical presence within a defined coordinate radius (such as 500 meters of Addis Ababa Stadium) before clearing them for competition.
- Club Roster and Transfer Registry: Club administrators must be able to view registered athletes, filter by license status, submit new registrations, and initiate transfer agreements with contract hashing.
- Automated Seeding Engine: The system must automatically group registered athletes into heats and assign lanes according to submitted qualifying marks.
- Multilingual Interface: Users must be able to switch between Amharic, Afaan Oromo, Tigrinya, and English without losing form state.

DevOps and Infrastructure Requirements:
- Multi-Tier Isolation: The system must isolate frontend, backend, and database components into discrete micro-containers.
- Persistent Database Storage: Runtime database transactions must persist across container destruction and restarts using named Docker storage volumes.
- Automated Quality Gating: Pull requests to production branches must trigger automated linting and test execution before merging.
- Decoupled Image Publishing: Successful builds must produce versioned images tagged with Git Commit SHAs stored in Docker Hub.
- Zero-Downtime Deployment: Code merges must trigger automated cloud deployment hooks without user interruption.
- Traffic Distribution and Telemetry: Ingress traffic must balance across backend replicas, with real-time CPU, memory, and HTTP metrics exposed to Prometheus and visualized in Grafana.

Non-Functional Requirements:
- Performance: Initial page bundle load times must remain below 1.5 seconds on standard 3G mobile connections.
- Security: Access tokens and refresh tokens must be securely stored, with automatic authorization header injection and silent renewal upon 401 Unauthorized responses.
- Responsiveness: The interface must render properly across smartphones, tablets, and desktop workstations.
- Reliability: Data must persist locally during network interruptions using browser storage fallback caches.

2.4.4. System Design and Multi-Container Architecture


The system design combines a multi-container cloud infrastructure with a modular application architecture. Below are the structural designs represented hierarchically:

#### Multi-Container System Architecture
```
system-architecture/
├── Ingress & Gateway Tier                  # Nginx reverse proxy, SSL/TLS termination (Port 80/443)
│   └── upstream_load_balancer              # Round-robin distributor balancing traffic across API replicas
├── Frontend Client Tier                    # React 18 / Vite SPA served via Nginx (Tailwind CSS, RTK Query)
├── Backend Application Tier                # Containerized Node.js / Express micro-services
│   ├── backend_replica_1 (Port 5000)       # Primary API instance handling auth, athlete, and club logic
│   └── backend_replica_2 (Port 5000)       # Secondary API instance ensuring high availability & failover
├── Database & Storage Tier (Port 5432)     # PostgreSQL 16 relational database engine
│   └── postgres_data_volume                # Persistent Docker named volume surviving container restarts
├── Inter-Container Network                 # Private virtual bridge network (eaf-bridge) with embedded DNS
└── Observability & Telemetry Stack         # Runtime telemetry and health monitoring
    ├── Prometheus Scraper                  # Time-series metrics collection (/metrics endpoint polling)
    └── Grafana Dashboards                  # Operational dashboards visualizing CPU, memory, and HTTP latency
```

#### Client Application Architecture (Source Tree)
```
src/
├── main.tsx                                # Application entry point, Redux Provider bootstrap
├── App.tsx                                 # Root component, router switchboard, modal coordinator
├── index.css                               # Global Tailwind utility directives and custom animations
├── i18n/ (am/, en/, or/, ti/)              # Multilingual catalogs (Amharic, English, Afaan Oromo, Tigrinya)
├── core/                                   # Shared foundation and cross-cutting infrastructure
│   ├── constants/, types/                  # Color tokens, theme definitions, TypeScript data models
│   ├── hooks/, utils/                      # Custom scroll hooks, Fayda ID validation formatters
│   └── data/ (mockData.ts)                 # Development datasets and offline testing fixtures
├── store/                                  # Centralized Redux Toolkit state architecture
│   ├── store.ts, persistence.ts            # Store config, typed hooks, LocalStorage cache sync
│   ├── slices/ (auth, athlete, club)       # Synchronous client state slices and RBAC claims
│   └── api/ (auth, athlete, events, news)  # RTK Query asynchronous data layer with auto token refresh
└── components/                             # Modular presentation tier and business feature views
    ├── LandingPage.tsx, Navbar.tsx         # Public landing hub, navigation header, language selector
    ├── Athlete/                            # Athlete management portal
    │   ├── AthleteOverview.tsx             # Performance trajectory and upcoming meet entries
    │   ├── FaydaVerification.tsx           # Fayda National Digital ID verification & biometrics
    │   ├── LicenseQrCode.tsx               # Dynamic 120s rotating QR competition credential
    │   └── GeofenceCheckin.tsx             # GPS-gated call room venue check-in (500m radius)
    ├── ClubAdmin/                          # Club administration portal
    │   ├── RosterManagement.tsx            # Member roster grid and license status tracking
    │   ├── TransferRegistry.tsx            # Inter-club transfer agreements & clearance locks
    │   └── SeedingGenerator.tsx            # Automated race seeding engine based on PB/SB marks
    ├── Modals/ (Auth, Register, Payment)   # Authentication, member onboarding, and payment modals
    └── Details/ (Athlete, Meet, News)      # Profile biographies, event schedules, and press releases
```

**Data Flow Sequence:** Ingress traffic enters at the Nginx Gateway, which terminates TLS and balances requests across the backend replicas. Each application replica executes business logic, queries PostgreSQL over the isolated Docker bridge network, and serves data to the client tier. Simultaneously, Prometheus and Grafana poll runtime telemetry across all active containers.


2.4.5. Technologies and Tools Used

The following technologies, developer libraries, and infrastructure tools were utilized throughout the project:

| Technology / Tool | Version | Purpose |
|---|---|---|
| React | 18.3.1 | Core UI component library and virtual DOM rendering engine |
| TypeScript | 5.5 / 7.0 | Static type definitions and compile-time error detection |
| Vite | 5.4.0 | Development server and production bundling build tool |
| Redux Toolkit | 2.12.0 | Centralized application state slices and reducer management |
| RTK Query | 2.12.0 | Asynchronous data fetching, query caching, and token refresh |
| Tailwind CSS | 3.4.19 | Utility-first responsive styling and theme token configuration |
| Framer Motion | 13.0.0 | Fluid UI transition effects and modal enter/exit animations |
| Lucide React | 1.27.0 | Scalable vector UI icons for sports, navigation, and badges |
| qrcode.react | 4.2.0 | Generation of dynamic call room credentials on SVG canvas |
| Docker & Compose | 26.x | Multi-container micro-service isolation and local orchestration |
| Docker Hub | Cloud | Centralized container registry for semantic and SHA-tagged images |
| GitHub Actions | v4 | Automated continuous integration, linting, and build validation |
| Nginx | Alpine | Ingress reverse proxy, upstream load balancing, and static hosting |
| Render & Vercel | Cloud | Production Continuous Deployment compute targets via deploy hooks |
| Prometheus | 2.50.x | Time-series metrics collection scraping engine endpoints |
| Grafana | 10.3.x | Operational analytics tracking and resource monitoring dashboards |
| VS Code & Git | Latest | Integrated code editing, branch protection, and commit tracking |

2.4.6. Implementation Phases, CI/CD Pipeline and Testing

Implementation followed three structured development phases across the twelve weeks:

Phase 1: Full-Stack CI/CD Infrastructure and Containerization (Weeks 1 to 4).
During the opening month, I implemented the foundational DevOps and orchestration layer based on the five-stage syllabus:
- Week 1 Milestone: Multi-Container Architecture. I decoupled the application into independent frontend, backend, and database tiers. In docker-compose.yml, services were joined across an internal bridge network. To guarantee data survival, the PostgreSQL container mounted a persistent named storage volume. I verified this by launching the stack locally with a single command: sudo docker compose up. After inserting test athlete records and destroying the containers, the database volume remounted. All records persisted intact.
- Week 2 Milestone: Docker Hub and Registry Management. Local container artifacts were migrated to remote registries. I adopted a deterministic image tagging topology using Git Commit SHAs (eaf-client:${GITHUB_SHA}) instead of mutable tags. Through authenticated CLI channels using Docker access tokens, compiled images were pushed to Docker Hub. I tested remote pulls on an isolated cloud virtual host, running the full stack without local source files.
- Week 3 Milestone: Continuous Integration (CI) Automation. I authored GitHub Actions workflows in .github/workflows/ci.yml. The pipeline triggers when pull requests target the production branch, running linting and unit tests in short-lived runner environments. Docker layer caching accelerated build speeds. When merged, the pipeline automatically compiles production images and pushes them to Docker Hub.
- Week 4 Milestone: Production Ingress, Load Balancing and Observability. I established automated deploy webhooks to Render and Vercel for zero-downtime rollouts. I configured an Nginx reverse proxy with an upstream block balancing traffic across two backend replicas. Concurrently, Prometheus scraped metrics from /metrics endpoints, populating Grafana dashboards with CPU, memory, and latency metrics.

Phase 2: Client Application Engineering and Federation Workflows (Weeks 5 to 10).
With the automated infrastructure active, I engineered the core application modules:
- In Week 5, I set up global Redux Toolkit slices in src/store/slices, persistence middleware, and base navigation components.
- In Week 6, I built FaydaVerification.tsx, verifying FIN inputs against Ethiopian identity hash indices under Proclamation No. 1284/2023.
- In Week 7, I completed RosterManagement.tsx and RegisterMember.tsx, allowing club administrators to track licensing statuses.
- In Week 8, I implemented TransferRegistry.tsx, adding federation clearance state machines and contract hashing.
- In Week 9, I developed SeedingGenerator.tsx, allocating track heats and lanes using Season Best and Personal Best sorting algorithms.
- In Week 10, I engineered GeofenceCheckin.tsx and LicenseQrCode.tsx, enforcing 500-meter GPS venue boundaries and 120-second dynamic QR refreshes.

Phase 3: Integration, Load Testing and Production Handover (Weeks 11 to 12).
In Week 11, I integrated multi-language dictionaries across Amharic, Afaan Oromo, Tigrinya, and English, applying custom CSS line-height fixes for Ge'ez fonts. In Week 12, I executed end-to-end integration tests, simulated concurrent user requests against the Nginx load balancer, and finalized technical documentation for federation staff.

2.4.7. Project Results

The completed system successfully merged modern user interface design with automated software delivery. All core features passed acceptance testing. Club managers verified rosters and submitted registrations without encountering UI errors. Race administrators generated seeded starter lists for the Addis Ababa Grand Prix within two seconds. Dynamic QR verification in the call room eliminated identity impersonation in simulated track trials.

On the operations side, the CI/CD pipeline eliminated manual deployment friction. Code pushed to GitHub passed through automated quality gates within three minutes. Production container images were published deterministically with Git Commit SHAs. The Nginx load balancer distributed traffic evenly between backend replicas, maintaining sub-second response times under concurrent request loads.

Chapter 3: Skills, Challenges, and Lessons Learned

3.1. Technical Skills Gained

The internship expanded my practical engineering capabilities across frontend and DevOps domains:

1. Modern Frontend Engineering: Gained deep expertise in React 18 component composition, custom hook development, and TypeScript generic constraints.
2. State Management and Data Caching: Configured Redux Toolkit stores, created modular slices, and wrote RTK Query endpoints with tag-based cache invalidation.
3. Multi-Container Orchestration: Mastered Docker multi-stage builds, compose file declarations, named storage volume management, and internal bridge network routing.
4. Registry and Image Tagging Topologies: Gained hands-on experience managing Docker Hub repositories, employing Git Commit SHA tagging to ensure deterministic rollbacks.
5. CI/CD Pipeline Construction: Authored GitHub Actions YAML workflows with layer caching, automated linting, test isolation, and secure secret injection.
6. Cloud Deployment and Zero-Downtime Rollouts: Configured deploy webhooks for Render and Vercel, managing environment variables outside source control.
7. Ingress Load Balancing: Configured Nginx upstream load balancing blocks to distribute requests across multiple backend container instances.
8. Time-Series Telemetry: Configured Prometheus metric scrapers and built analytical tracking dashboards in Grafana to monitor system resource allocations.

3.2. Software Engineering and DevOps Practices

Working inside an active software division at INSA exposed me to professional engineering workflows. Our team followed Scrum methodologies with two-week sprint cycles. We tracked tasks using digital boards, broke broad features into atomic user stories, and held daily standups.

DevOps discipline reshaped how our team handled code changes. We enforced strict branch protection rules that prohibited direct commits to main branches. Pull requests required passing automated CI verification gates before peer review. Commit messages followed conventional standards, ensuring an auditable git history. Decoupling configuration from application code through environment variables prevented credential leaks.

3.3. Professional Skills Gained

In addition to technical proficiencies, the internship strengthened my workplace competencies:

Communication skills improved substantially. I learned to explain technical trade-offs to federation sports officials who were unfamiliar with web development terms. Project management abilities developed as I estimated task durations and prioritized bug fixes under fixed sprint deadlines. Pair programming sessions taught me how to give and receive constructive code feedback during pull request reviews.

3.4. Application of Academic Knowledge

The internship allowed me to put theoretical knowledge from the AASTU Software Engineering curriculum into practice:

- Object-Oriented Programming (OOP): Applied encapsulation and inheritance principles when defining TypeScript interfaces, typing Redux action payloads, and structuring reusable UI components.
- Database Systems: Relational modeling concepts guided the normalization of client data structures, foreign key constraints, and managing persistent PostgreSQL storage volumes.
- Data Structures and Algorithms: Designed the seeding algorithm in SeedingGenerator.tsx using sorting algorithms and priority queues to allocate athletes to heats based on Season Best times.
- Software Engineering: Applied requirement elicitation techniques, created UML sequence flows for authentication, and documented functional specifications.
- Internet Programming: Leveraged knowledge of the HTTP protocol, RESTful endpoints, browser storage mechanisms, and asynchronous promise handling.
- Software Architecture: Implemented layered architecture patterns that separate presentation views from state stores, network services, and containerized backend microservices.
- Data Communication and Computer Networks: Applied principles of reverse proxies, CORS policies, DNS routing, Docker bridge networks, and Nginx round-robin load balancing.
- Operating Systems: Applied understanding of Linux process management, namespaces, cgroups, file permissions, named volumes, and signal handling inside Docker containers.
- Software Testing: Used verification concepts learned in class to write test cases, perform boundary value analysis on form inputs, and build isolated CI test runners in GitHub Actions.

3.5. Challenges Encountered

3.5.1. Technical Challenges

During development, several technical obstacles emerged:

First, token expiration caused race conditions. When multiple simultaneous API queries returned 401 Unauthorized status codes, multiple refresh token requests were fired concurrently. This invalidated the token on the server and forced unexpected user logouts.

Second, mobile browser geolocation accuracy varied significantly. When testing the geofence check-in component on physical phones, GPS coordinate drift frequently placed athletes outside the 500-meter stadium perimeter even when they were standing on the track.

Third, database state was inadvertently wiped during early container restarts. Without explicit named volume declarations, running docker compose down destroyed runtime athlete records.

Fourth, GitHub Actions build times were initially slow. Redownloading npm dependencies and rebuilding base Docker layers on every push created seven-minute pipeline delays.

Fifth, Prometheus failed to scrape backend metrics initially because container hostnames were not resolvable outside the Docker internal bridge network.

3.5.2. Workplace Challenges

Understanding the detailed rulebook of World Athletics and EAF competition regulations was difficult at first. Terminology surrounding transfer windows, false start disqualifications, and age tier thresholds required repeated clarification meetings with federation officers. Frequent backend API endpoint contract updates also required immediate adjustments to client-side data schemas.

3.6. Solutions Applied

The challenges were systematically resolved through targeted technical solutions:

| Challenge | Solution Applied |
|---|---|
| Concurrent token refresh requests causing session logouts | Implemented a mutex lock inside apiSlice.ts. The base query pauses outbound requests during an active refresh cycle and retries them sequentially once the new bearer token arrives. |
| Geolocation coordinate drift on mobile devices | Integrated a rolling average filter across three consecutive GPS readings and added a 50-meter tolerance buffer to the haversine distance evaluation. |
| Database transaction loss during container tear-downs | Defined an explicit named volume in docker-compose.yml for PostgreSQL data storage, decoupling database state from container lifecycles. |
| Slow pipeline execution in GitHub Actions | Implemented GitHub Actions layer caching for Docker builds and cache actions for npm dependencies, reducing pipeline run times from seven minutes to two minutes. |
| Prometheus scraping failures across container boundaries | Configured service name DNS resolution within a shared Docker bridge network, pointing Prometheus directly to backend internal container endpoints. |
| Shifting backend API contracts during development | Built a dedicated mock service worker layer and strict TypeScript interface contracts, allowing frontend work to proceed without dependency on backend deployment timing. |
| Complex athletic transfer rules and clearance delays | Constructed a dedicated transfer state machine with explicit status flags (Pending, Federation Locked, Approved, Rejected) and real-time status badges. |

3.7. Lessons Learned

The internship yielded key engineering lessons. First, deterministic state management is essential in distributed web applications. Uncontrolled side effects create bugs that are difficult to reproduce. Second, DevOps automation is an essential part of software engineering. Manual deployments are prone to human error, whereas automated CI/CD pipelines provide repeatability and speed. Third, user experience in public infrastructure systems must prioritize simplicity and accessibility. Athletes accessing the portal from basic mobile devices on cellular networks require lightweight pages that render without delay. Fourth, observability is vital in production systems. Without Prometheus metrics and Grafana dashboards, diagnosing performance bottlenecks requires guesswork.

Chapter 4: Conclusion and Recommendations

4.1. Conclusion

The internship at INSA in collaboration with the Ethiopian Athletics Federation provided an in-depth software engineering experience. Over twelve weeks, I applied theoretical concepts learned at AASTU to engineer the EOSCRMS portal and establish its production delivery infrastructure.

The project succeeded in establishing containerized multi-tier orchestration, GitHub Actions CI pipelines, zero-downtime cloud deployments, Nginx load balancing, and Grafana monitoring during the initial month. Subsequent weeks delivered digital athlete identity verification via Fayda National ID, dynamic QR licensing, club roster management, and algorithmic competition seeding. The experience broadened my technical abilities in React, TypeScript, Redux Toolkit, Docker, Nginx, Prometheus, and CI/CD pipelines, while reinforcing the value of professional teamwork and discipline.

4.2. Recommendations to the Host Organization

Based on my observations during development, I offer the following recommendations to INSA and EAF:

1. Implement Automated Container Security Scanning: Integrate vulnerability scanning tools such as Trivy or Snyk into the GitHub Actions pipeline to detect vulnerable base images and dependencies before container publication.
2. Expand Telemetry Alerting Channels: Configure Prometheus Alertmanager to broadcast automated alerts to Telegram or SMS channels when server error rates or CPU thresholds exceed safe limits.
3. Implement Offline-First Service Workers: Athletes and coaches often train in remote highland areas with unreliable cellular connectivity. Adding service workers with background synchronization will allow users to log training data offline and synchronize when connectivity resumes.
4. Expand Automated End-to-End Testing: While unit and lint checks are active, establishing end-to-end browser testing using Playwright will safeguard critical user journeys such as competition registration and fee payment flows.

4.3. Recommendations to the Department of Software Engineering

To strengthen the Software Engineering program at AASTU, I recommend the following enhancements:

1. Incorporate Full-Stack DevOps and CI/CD into the Core Curriculum: Establish practical lab exercises teaching Dockerfile creation, compose multi-service orchestration, and GitHub Actions workflow writing within Internet Programming and Software Architecture courses.
2. Adopt TypeScript Early: Modern software engineering relies on typed JavaScript. Teaching TypeScript in place of vanilla JavaScript will better prepare students for contemporary industry requirements.
3. Teach Production Observability: Introduce students to time-series monitoring, log aggregation, and Grafana dashboard design so they understand system maintenance after deployment.

4.4. Recommendations to Future Students

For students preparing to undertake their internships, I offer the following advice:

1. Learn Docker and Containerization Before Your Internship: Understanding how to containerize an application and orchestrate services locally provides an immediate advantage in professional teams.
2. Master Version Control and Git Early: Learn how to manage merge conflicts, write clean commit messages, configure branch protection rules, and review pull requests.
3. Build Real Full-Stack Applications: Hands-on experience connecting frontends, backends, databases, and CI pipelines builds genuine confidence and technical capability.
4. Communicate Proactively: Do not hesitate to ask questions when requirements are ambiguous. Regular communication with senior engineers prevents wasted effort and accelerates learning.

Annex

Annex A: System Navigation and Core Routes

| Route Path | Associated Component | Primary User Role | Purpose |
|---|---|---|---|
| / | LandingPage.tsx | Public / Guest | Federation announcements, race calendar, and portal login access |
| /athlete/overview | AthleteOverview.tsx | Athlete | Personal sports profile, personal best records, and license status |
| /athlete/fayda | FaydaVerification.tsx | Athlete | Fayda National ID verification and secondary KYC upload |
| /athlete/checkin | GeofenceCheckin.tsx | Athlete | GPS venue verification and 120-second dynamic QR call room badge |
| /athlete/events | AthleteEvents.tsx | Athlete | Competition registration and heat status tracking |
| /club/overview | ClubOverview.tsx | Club Admin | Club squad metrics, licensed counts, and regional ranking |
| /club/roster | RosterManagement.tsx | Club Admin | Member list filtering, licensing renewal, and athlete profile views |
| /club/transfers | TransferRegistry.tsx | Club Admin | Inter-club transfer submissions and federation clearance tracking |
| /club/seeding | SeedingGenerator.tsx | Club Admin / Marshal | Automated lane and heat seeding using SB/PB athlete metrics |

Annex B: End-to-End CI/CD Pipeline Workflow

```
[ Developer Branch ]
         |
         | (git push / Pull Request)
         v
[ GitHub Actions CI Runner ]
         |
         +---> Step 1: Code Checkout & Environment Setup
         +---> Step 2: Automated Linting (ESLint 9.8)
         +---> Step 3: Automated Test Execution Isolation
         +---> Step 4: Docker Layer Caching & Multi-Stage Build
         |
   (PR Merged to Main)
         |
         v
[ Docker Hub Container Registry ]
         | (Tagged with Git Commit SHA)
         |
         +---------------------------------------+
         |                                       |
         v                                       v
[ Render / Vercel Cloud CD ]            [ Production Host VM ]
   (Automated Webhook Deploy)               (Nginx Reverse Proxy)
   (Zero-Downtime Traffic Switch)           (Upstream Load Balancer)
         |                                       |
         v                                       v
[ Live Public Web App ]                 [ Prometheus & Grafana ]
```