# StudyCRM Onboarding & Authorization Flow

A clean, high-level workflow of the StudyCRM multi-tenant lifecycle from platform administration to scoped staff authorization.

---

## 1. Clean Workflow Flowchart

```mermaid
flowchart TD
    %% Modern, Minimalist Styling
    classDef platform fill:#f5f3ff,stroke:#7c3aed,stroke-width:2px,color:#4c1d95;
    classDef company fill:#eff6ff,stroke:#2563eb,stroke-width:1.5px,color:#1e40af;
    classDef review fill:#fffbeb,stroke:#d97706,stroke-width:1.5px,color:#92400e;
    classDef staff fill:#f0fdf4,stroke:#16a34a,stroke-width:1.5px,color:#14532d;
    classDef finish fill:#dcfce7,stroke:#15803d,stroke-width:2px,color:#14532d;

    subgraph P1 ["Phase 1: Platform & Registration"]
        S1(["1. StudyCRM Dev Admin"]):::platform
        --> S2["2. Company Registration"]:::company
        --> S3["3. Email Verification"]:::company
        --> S4["4. Pending Review"]:::review
        --> S5["5. Dev Admin Approval"]:::review
    end

    subgraph P2 ["Phase 2: Company Setup"]
        S5 --> S6["6. Company Initialization"]:::company
        --> S7["7. Company Owner Login"]:::company
        --> S8["8. Create Branch"]:::company
        --> S9["9. Invite Staff"]:::company
        --> S10["10. Assign Role + Scope"]:::company
    end

    subgraph P3 ["Phase 3: Staff Activation & Access"]
        S10 --> S11["11. Staff Accepts Invitation"]:::staff
        --> S12["12. Staff Login"]:::staff
        --> S13(["13. Authorization Works"]):::finish
    end
```

---

## 2. Quick 1:1 Flow Reference

```mermaid
flowchart TD
    A["StudyCRM Dev Admin"]
    --> B["Company Registration"]
    --> C["Email Verification"]
    --> D["Pending Review"]
    --> E["Dev Admin Approval"]
    --> F["Company Initialization"]
    --> G["Company Owner Login"]
    --> H["Create Branch"]
    --> I["Invite Staff"]
    --> J["Assign Role + Scope"]
    --> K["Staff Accepts Invitation"]
    --> L["Staff Login"]
    --> M["Authorization Works"]

    style A fill:#f5f3ff,stroke:#7c3aed,stroke-width:2px
    style E fill:#fffbeb,stroke:#d97706,stroke-width:2px
    style F fill:#eff6ff,stroke:#2563eb,stroke-width:2px
    style J fill:#f0fdf4,stroke:#16a34a,stroke-width:2px
    style M fill:#dcfce7,stroke:#15803d,stroke-width:2px
```

---

## 3. Flow-Wise Stage Summary

| # | Step | Actor | Summary |
| :-: | :--- | :--- | :--- |
| **1** | **StudyCRM Dev Admin** | Platform Admin | Manages SaaS platform, plans, and tenant review queues. |
| **2** | **Company Registration** | Agency | Submits agency name, work email, phone, and admin credentials. |
| **3** | **Email Verification** | System / User | Verifies email ownership via a 6-digit OTP code (5-min TTL). |
| **4** | **Pending Review** | System | Account enters audit queue with `PENDING_REVIEW` status. |
| **5** | **Dev Admin Approval** | Platform Admin | Dev Admin reviews agency credentials and approves tenant creation. |
| **6** | **Company Initialization** | System | Provisions isolated tenant database (`agency_id`), default roles, and settings. |
| **7** | **Company Owner Login** | Agency Owner | Owner logs into dedicated workspace with `Agency Admin` privileges. |
| **8** | **Create Branch** | Agency Owner | Sets up operating branches (e.g., Head Office, Regional Branch). |
| **9** | **Invite Staff** | Agency Owner | Sends single-use email invitation link to staff members. |
| **10** | **Assign Role + Scope** | Agency Owner | Assigns functional role (Counselor, Admissions) + scope (Branch, Global). |
| **11** | **Staff Accepts Invitation** | Staff User | Staff opens secure link, verifies identity, and creates their password. |
| **12** | **Staff Login** | Staff User | Staff logs in; system builds JWT session with assigned branch and scopes. |
| **13** | **Authorization Works** | Security Gateway | RBAC rules and data isolation filters are actively enforced on every action. |
