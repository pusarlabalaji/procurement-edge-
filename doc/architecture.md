

Purpose

    Give a non-technical picture of how the app is organized and how data moves between parts.

Simple block diagram (ASCII)

 block below. It’s easy to scan and change later.

Block diagram (paste into architecture.md):

    Architecture (high level)

        Presentation layer

            Buyer Web App

            Supplier Web App

        Services layer

            Benchmark Service

            Landed-Cost Engine

            Compliance Service

            Exchange Match Service

        Data and AI layer

            Market Rates DB

            Supplier/Performance Store

            Tariff/Rules Catalog

            Feature Store + Models

        Integration layer

            ERP/MRP Connectors

            Logistics/Customs APIs

            Identity/SSO

        Cross-cutting

            Audit Logs

            Notifications

            Analytics

ASCII layout:
+----------------------- Presentation -----------------------+
| Buyer Web App | Supplier Web App |
+----------------------------+-------------------------------+
| |
+------------------------ Services --------------------------+
| Benchmark | Landed-Cost | Compliance | Exchange Match |
+----------------------------+-------------------------------+
| |
+------------------------ Data & AI -------------------------+
| Market Rates | Supplier Store | Tariff Rules | Feature/ML |
+----------------------------+-------------------------------+
| |
+----------------------- Integrations -----------------------+
| ERP/MRP | Logistics/Customs | Identity/SSO |
+-----------------------------------------------------------+
^ Observability/Audit v
+------------------------ Cross-cutting --------------------+
| Logs | Metrics | Alerts | Notifications |
+-----------------------------------------------------------+

    Data flow (plain steps)

        Buyer enters SKU/BOM and trade details → Landed-Cost Engine fetches tariff/duty info → calculates total cost → displays with benchmark price bands.

Buyer creates RFQ → Exchange Match Service routes to suppliers → Compliance Service checks certifications → responses return to buyer.
Purpose

This document explains the high‑level architecture of the procurement intelligence app, how the main parts fit together, and how data flows from ERP files to insights and RFQs. Keep it short and focused on what code cannot easily show.
Audience

Product, engineering, and onboarding stakeholders who need a quick, non‑deep technical picture of the system.
System overview

The platform has four core capabilities: AI Benchmarks, Landed‑Cost Calculator, Auto Compliance, and Smart Exchange. It integrates with ERPs for supplier, PO, and invoice data. Use screenshots to ground the outcomes and the ERP path:

    Proven results metrics: see screenshots/05-proven-results.png.

ERP data upload flow and supported file formats: see screenshots/06-erp-integration.png.
Block diagram
 It communicates components, layers, and relationships clearly without tooling. Keep it updated as the system evolves.

+----------------------- Presentation -----------------------+
| Buyer Web App | Supplier Web App |
+----------------------------+-------------------------------+
| |
+------------------------ Services --------------------------+
| Benchmark | Landed-Cost | Compliance | Exchange Match |
+----------------------------+-------------------------------+
| |
+------------------------ Data & AI -------------------------+
| Market Rates | Supplier Store | Tariff Rules | Feature/ML |
+----------------------------+-------------------------------+
| |
+----------------------- Integrations -----------------------+
| ERP/MRP | Logistics/Customs | Identity/SSO |
+-----------------------------------------------------------+
^ Observability/Audit v
+------------------------ Cross-cutting --------------------+
| Logs | Metrics | Alerts | Notifications |
+-----------------------------------------------------------+

Notes:

    Components show the building blocks; connectors represent relationships and data exchange.

Keep abstraction high; avoid code-level detail here. Add deeper diagrams only when necessary.
Data flow (happy path)

    Import procurement data from ERP exports (Excel, CSV, PDF, or direct exports) via secure upload. The Integrations layer normalizes and validates records. See 06-erp-integration.png for supported formats.

Benchmark Service aggregates market rates and computes price bands for items/categories; results feed UI and negotiations.

Landed‑Cost Engine calculates total landed cost using tariffs, duties, freight, and fees; outputs optimizations to the UI.

Exchange Match Service routes RFQs to qualified suppliers; responses return to buyers.

Compliance Service checks certifications and risk; status gates supplier participation.

Proven results (savings, cost reduction, speed, coverage) are reflected in 05-proven-results.png.
Quality attributes

    Scalability: services scale independently; batch ERP ingestion decoupled from interactive workloads.

Security: SSO for portals; encrypted uploads; role-based access for modules.

Observability: centralized logs/metrics/alerts; audit trails for calculations and RFQ events.
Key decisions (record briefly, link to ADRs)

    Adopt layered services with separate Benchmark, Landed‑Cost, Compliance, and Exchange services to isolate concerns.

Store docs and diagrams alongside code in /docs, updating with each release.
How to evolve the diagram

    Start with context and layers; refine with container/deployment views as needed (C4 style). Keep diagrams lightweight and purpose-driven.

Validate with 2–3 real scenarios (e.g., “import PO history,” “calculate landed cost,” “issue RFQ”). Iterate based on feedback.
Appendix

    Screenshots

        screenshots/05-proven-results.png — outcomes and metrics used in value narrative.

screenshots/06-erp-integration.png — ERP upload steps and file support.

Update rule: if a component or flow changes, update the ASCII diagram and the affected bullet list in this file
