# System Architecture & Engineering Case Studies

A curated collection of NDA-safe architectural breakdowns, infrastructure topologies, and production engineering patterns across multi-tenant SaaS platforms, AI orchestration pipelines, cross-platform mobile apps, reactive single-page applications, and high-performance client interfaces.

## Purpose & Methodology
Each document abstracts proprietary business logic and client identities to focus strictly on **system design, scalability trade-offs, security boundaries, and automated quality assurance**. Diagrams are authored in Mermaid.js for native rendering and machine readability.

---

## Case Studies Index

### 1. [Multi-Tenant Omnichannel Messaging & Automation Platform](./multi-tenant-messaging-backend.md)
* **Domain:** Distributed SaaS Backend & Real-Time Event Processing
* **Core Stack:** PHP 8.4, Laravel 13, MariaDB (Database-per-tenant), Redis, Laravel Horizon, Soketi (WebSockets), PHPUnit, Playwright.
* **Key Architectural Patterns:** Database-per-tenant isolation with a central control plane, decoupled webhook ingress rate-limiting, workload-specific Redis queues, and event-driven real-time broadcasting.

### 2. [Self-Hosted AI Orchestration & RAG Platform](./ai-orchestration-platform.md)
* **Domain:** AI Control Plane, Vector Retrieval & Automated Workflows
* **Core Stack:** PHP 8.3+, Laravel 13 (AI SDK, Reverb), React 18, TypeScript, PostgreSQL 18, Qdrant Vector DB, Redis, Python (FastAPI), Docker.
* **Key Architectural Patterns:** Provider-agnostic AI runtime abstraction, dedicated vector-store document lifecycle (ingestion/indexing/scoping), queue-backed workflow execution, and dual authentication (Sanctum + API Keys).

### 3. [Omnichannel Customer Engagement Workspace](./omnichannel-frontend-workspace.md)
* **Domain:** Enterprise Single-Page Application & Embeddable Client SDKs
* **Core Stack:** React 18, Redux Toolkit, TanStack Query, Material UI, Vite, Laravel Echo, Playwright.
* **Key Architectural Patterns:** Domain-sliced client state management, runtime tenant-aware API resolution, decoupled WebSocket subscription lifecycles, and standalone IIFE embeddable widget builds.

### 4. [LAN Media Playback & Casting Client (Flutter Mobile App)]('./lan-media-casting-client.md)
* **Domain:** Cross-Platform Mobile Engineering, Local Network Protocols & Edge Streaming
* **Core Stack:** Dart 3, Flutter, Riverpod, `go_router`, `shelf` (Local HTTP Proxy), mDNS/DLNA helper libraries, native Android background services.
* **Key Architectural Patterns:** Protocol adapter pattern for heterogeneous casting receivers, device-local HTTP media proxying, heuristic URL classification for web streams, and app-scoped asynchronous queue/cache states.

### 5. [Static Media Product Experience](./static-media-product-experience.md)
* **Domain:** High-Performance Zero-Backend Web Engineering
* **Core Stack:** Vanilla JavaScript (ES6+), HTML5, Tailwind CSS v4, Vite.
* **Key Architectural Patterns:** Zero-server attack surface, browser-native localization with `localStorage`, accessible progressive enhancement (`IntersectionObserver`, `prefers-reduced-motion`), and subpath-aware asset bundling.

---

*Authored by **Heriberto H.** — Senior Full-Stack & Systems Engineer ([Portfolio](https://heribertolord96.github.io/) | [GitHub](https://github.com/heribertolord96))*
