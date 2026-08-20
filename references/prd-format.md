# Complete Guide: 7-Section PRD Structure

> **Called from**: `SKILL.md`, Stage 1.
> **Contents of this file**: Precise markdown template for the PRD (Overview, Requirements, Core Features, User Flow, Architecture, Database Schema, Design & Technical Constraints), including sample Mermaid diagrams and typography rules.
> **Read this file BEFORE generating the PRD** — follow the structure & section order exactly as in the example, adapting the content to the user's project context.

---

## Mandatory PRD Format & Structure

Every PRD produced MUST follow these 7 main sections precisely:

```markdown
# PRD — Product Requirements Document: [Product / App Name]

## 1. Overview
### Background & Core Problem
[Describe the problem to solve, e.g. digitizing manual records, tracking real-time stock, shelf location, batch number]

### Main Product Goals
[Describe the app's vision, target platform (Web/Mobile), target users (e.g. Single Admin), and expected outcomes]

---

## 2. Requirements
List the system's high-level requirements:
- **Accessibility**: [Devices & platform access, e.g. Desktop/Laptop web browser]
- **Users**: [User access & roles, e.g. Single Admin with full access]
- **Data Input**: [Data entry methods, e.g. Manual typed input vs Barcode scan]
- **Data Specifics**: [Important per-product attribute details, e.g. Batch Number, Shelf Location]
- **Notifications**: [Notification/alert mechanism, e.g. Visual Low Stock Alert on Dashboard]

---

## 3. Core Features
List of key features for the MVP (Minimum Viable Product):

### 1. Main Dashboard
- Summary of total product count and asset value (optional).
- Low Stock Alert panel: list of products below the minimum threshold.

### 2. Product Management (Master Data)
- Add, Edit, and Delete Products (CRUD).
- Required fields: Product Name, SKU, Unit, Shelf Location, Minimum Stock.

### 3. Inbound Stock Entry
- Form for adding stock.
- Required inputs: Select Product, Quantity, Batch Number, Received Date.

### 4. Outbound Stock Entry
- Form for reducing stock.
- Required inputs: Select Product, Quantity, Select Batch (manual/auto FIFO/LIFO), Notes.

### 5. Movement Log (History Report)
- Transaction history table.
- Attributes: Admin (User), Timestamp, Product, Quantity (In/Out), Batch, Notes.

---

## 4. User Flow
Step-by-step user workflow:

1. **Login**: Admin signs in with email and password.
2. **Monitoring**: Admin checks the Dashboard to monitor stock status & Low Stock Alerts.
3. **Product Setup (Initial)**: Admin enters new product data when there are new items (including SKU & Shelf Location).
4. **Update Stock**:
   - **Inbound**: Admin opens the "Inbound" menu → selects product → fills in quantity & batch number → saves.
   - **Outbound**: Admin opens the "Outbound" menu → selects product & batch → fills in quantity → saves.
5. **Verification**: The system automatically updates total & batch remaining stock, and records the transaction in Movement Logs.

---

## 5. Architecture
Overview of the system architecture and technical data flow:

```mermaid
graph TD
    User([Admin User]) <-->|HTTPS / UI| FE[Frontend Web App]
    FE <-->|REST API / JSON| BE[Backend Application Logic]
    BE <-->|ORM / SQL| DB[(Database)]

    subgraph Data Flow
        FE -->|Submit Stock In/Out| BE
        BE -->|Validate & Update Batch| DB
        DB -->|Return Updated Stock| BE
        BE -->|Update Low Stock Alert| FE
    end
```

Describe the components:
- **Frontend Layer**: Responsive web interface (Desktop/Laptop prioritized).
- **Backend Service**: Business logic, stock validation, batch management, movement logging.
- **Database Layer**: Relational data persistence (PostgreSQL / MySQL / SQLite).

---

## 6. Database Schema
Entity Relationship Diagram (ERD) structure and table descriptions:

```mermaid
erDiagram
    users ||--o{ stock_movements : "creates log"
    products ||--o{ batches : "has many"
    products ||--o{ stock_movements : "records movement"
    batches ||--o{ stock_movements : "associated with"

    users {
        uuid id PK
        string email
        string password_hash
        string name
        timestamp created_at
    }

    products {
        uuid id PK
        string sku UK
        string name
        string unit
        string shelf_location
        integer min_stock
        timestamp created_at
        timestamp updated_at
    }

    batches {
        uuid id PK
        uuid product_id FK
        string batch_number
        integer quantity
        date received_date
        timestamp created_at
    }

    stock_movements {
        uuid id PK
        uuid product_id FK
        uuid batch_id FK
        uuid user_id FK
        string movement_type "IN / OUT"
        integer quantity
        string notes
        timestamp created_at
    }
```

### Database Table Summary
| Table | Description |
| :--- | :--- |
| `products` | Product master data (SKU, unit, shelf location, minimum stock threshold). |
| `batches` | Per-batch inbound record per product with unique batch number & remaining batch stock. |
| `stock_movements` | Stock movement transaction log (in/out), linked to product, batch, and admin. |
| `users` | Admin account data with system access rights. |

---

## 7. Design & Technical Constraints
UI/UX design guidelines and technical constraints:

> ⚠️ **For projects with UI**: fill the Typography Rules & Color Tokens below from the "UI/UX Reference" sub-flow result (see `references/interview-guide.md`) — NOT from the generic example below. If the user provided their own reference, use the tokens extracted from that reference. If the user picked a preset, copy the preset tokens exactly from `references/design-system-presets.md` (hex color, font name, radius — do not paraphrase into vague terms). The example below is purely illustrative of the format, not a default to be used as-is.

### High-Level Technology
- Use modern technology that supports rapid development and easy maintenance.
- Flexible toward the framework/tech stack (not strictly bound), but prioritizes performance, efficiency, and scalability for small to medium scale.

### Typography Rules
Mandatory font variable rules for the UI (filled from the chosen reference/preset):
- **Sans**: `Geist Mono, ui-monospace, monospace`
- **Serif**: `serif`
- **Mono**: `JetBrains Mono, monospace`

### Color Tokens
Filled from the chosen reference/preset — include at minimum Primary, Accent, and semantic colors (Success/Warning/Danger).

### UI & Layout Rules
- **Look & feel**: Dashboard-driven, clean visual hierarchy, high-contrast alert status.
- **Alert availability**: High-contrast colored badge/card (e.g. red/orange) for products that reach or fall below `min_stock`.
```
