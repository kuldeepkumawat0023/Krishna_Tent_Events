# Krishna Tent & Events ERP - Agent Rules

## Project Overview

This is a **Warehouse-Centric Event ERP** built for **Krishna Tent & Events** — a single-owner tent and event management business with **multiple warehouses (godowns)** (dynamically managed by the owner). This is NOT a SaaS product. No subscription, no multi-tenant, no company registration flow. The owner manages tent bookings, material movement, event execution, and finance through this ERP.

**Core Philosophy:** WAREHOUSE → AVAILABLE STOCK → EVENT BOOKING → RESERVATION → LOADING → DISPATCH → SITE → RETURN → VERIFICATION → AVAILABLE AGAIN

## Tech Stack

| Layer        | Technology                        |
|-------------|-----------------------------------|
| Frontend    | Next.js 16 (v16.2.5, App Router) + TypeScript |
| Styling     | Tailwind CSS v4 + shadcn/ui       |
| Backend     | Node.js + Express.js              |
| Database    | MongoDB + Mongoose ODM            |
| Auth        | JWT + bcrypt                      |
| State       | React Context + SWR               |
| File Upload | Multer + Cloudinary               |
| AI Features | DeepSeek API                      |
| Real-time   | Socket.io                         |

## Project Structure

```
Krishna Tent & Events ERP/
├── frontend/                 # Next.js 16 App (TypeScript)
│   └── src/
│       ├── proxy.ts          # API proxy configuration
│       ├── app/              # App Router — ONLY page.tsx route files
│       │   ├── (auth)/       # Auth routes (login, setup-wizard)
│       │   └── (dashboard)/  # Dashboard routes (all modules)
│       ├── components/       # ALL UI component logic
│       │   ├── auth/         # Auth components (LoginForm, AuthGuard, etc.)
│       │   ├── common/       # Shared UI (DataTable, Card, Button, etc.)
│       │   └── dashboard/    # Module components in *-group/ sub-folders
│       │       ├── dashboard/         # DashboardView, CalendarWidget
│       │       ├── crm-group/         # customers/, leads/, site-visits/
│       │       ├── quotation-group/   # QuotationForm, ItemSelector
│       │       ├── booking-group/     # BookingForm, AgreementView
│       │       ├── warehouse-group/   # warehouses/, transfer/
│       │       ├── inventory-group/   # categories/, items/, ledger/
│       │       ├── operations-group/  # reservation/, dispatches/, events/
│       │       ├── finance-group/     # payments/, expenses/, invoices/, cashbook/, bankbook/
│       │       ├── purchases-group/   # purchases/, vendors/
│       │       ├── hr-group/          # staff/, attendance/, vehicles/
│       │       ├── layout/            # DashboardLayout, SideNavBar, TopNavBar, WarehouseSwitcher, NotificationBell
│       │       ├── calendar/          # EventCalendar
│       │       ├── gallery/           # PhotoGrid, EventGalleryView
│       │       ├── reports/           # All report views
│       │       └── settings/          # Company, Preferences, Roles, Users
│       ├── contexts/         # React Context providers (AuthContext, SocketContext)
│       ├── hooks/            # Custom hooks (useAuth, usePermissions, useDebounce, useChatSocket, useSocket)
│       ├── lib/              # API Client + services/ + constants/
│       ├── provider/         # ThemeProvider, HydrationGuard, StoreProvider, ChatProvider
│       ├── store/            # Redux Store + chatSlice + authSlice
│       └── utils/            # Helpers (cn, formatCurrency, validations)
│
├── backend/                  # Express.js API (Node.js)
│   ├── src/
│   │   ├── config/           # DB connection, Cloudinary, DeepSeek, Socket, Email config
│   │   ├── controllers/      # Business logic controllers
│   │   ├── middlewares/      # Auth, RBAC, CSRF, RateLimiter, Upload, Validator
│   │   ├── models/           # Mongoose schemas (all collections)
│   │   ├── routes/           # Express route handlers (*Routes.js) + index.js master registry
│   │   ├── services/         # Service layer (excel, pdf, sync, stock logic, ERP engine)
│   │   ├── utils/            # Helper functions (apiError, formatHelpers, logger, pdf)
│   │   └── app.js            # Express app configuration & middleware mount
│   ├── tests/                # Jest API unit & integration tests
│   ├── package.json
│   └── server.js             # Server listener & DB startup
│
├── packages/
│   └── shared/               # Shared types & constants
│
└── project_guide/            # Documentation & Reference Architecture (DO NOT MODIFY)
```

## Frontend Architecture Rules

### Key Rule: `app/` = Routes ONLY, `components/` = ALL UI Logic

- **`app/` folder** contains ONLY `page.tsx` and `layout.tsx` files — no component logic, no forms, no views
- **`components/dashboard/*-group/`** contains ALL UI components organized in sub-folders per module
- Each module sub-folder follows the pattern: `ListView.tsx` + `DetailView.tsx` + `Form.tsx`

### Component Group → Sub-folder Mapping

| Component Group | Sub-folders | Key Components |
|----------------|-------------|----------------|
| `crm-group/` | `customers/`, `leads/`, `site-visits/` | CustomerForm, LeadForm, SiteVisitForm |
| `quotation-group/` | (flat) | QuotationForm, ItemSelector, StockAvailabilityCheck |
| `booking-group/` | (flat) | BookingForm, AgreementView |
| `warehouse-group/` | `warehouses/`, `transfer/` | WarehouseForm, ZoneManager, RackManager, StockTransferForm |
| `inventory-group/` | `categories/`, `items/`, `ledger/` | ItemForm, CategoryForm, LedgerView |
| `operations-group/` | `reservation/`, `dispatches/`, `events/` | ReservationForm, DispatchForm, SiteReceiptForm, ReturnForm, VerificationForm |
| `finance-group/` | `payments/`, `expenses/`, `invoices/`, `cashbook/`, `bankbook/` | PaymentForm, ExpenseForm, InvoiceBuilder |
| `purchases-group/` | `purchases/`, `vendors/` | PurchaseForm, VendorForm |
| `hr-group/` | `staff/`, `attendance/`, `vehicles/` | StaffForm, AttendanceView, VehiclesView |

### Route → Component Import Pattern

```tsx
// app/(dashboard)/crm/customers/page.tsx — ONLY imports & renders
import { CustomersView } from '@/components/dashboard/crm-group/customers/CustomersView';
export default function CustomersPage() {
  return <CustomersView />;
}
```

## Backend API Routes (30 Route Modules)

All routes follow pattern: `/api/<module>/<action>` registered in `backend/src/routes/index.js`

| Route File | Base Path | Key Endpoints |
|-----------|-----------|---------------|
| `authRoutes.js` | `/api/auth` | login, logout, refresh, me |
| `dashboardRoutes.js` | `/api/dashboard` | stats, today-events, alerts |
| `crmRoutes.js` | `/api/crm` | customers CRUD, leads CRUD, site-visits |
| `quotationRoutes.js` | `/api/quotations` | CRUD, approve, reject, PDF |
| `bookingRoutes.js` | `/api/bookings` | CRUD, agreement, advance |
| `warehouseRoutes.js` | `/api/warehouses` | CRUD, zones, racks, stock-summary |
| `warehouseTransferRoutes.js` | `/api/warehouse-transfers` | create, approve, dispatch, receive |
| `inventoryRoutes.js` | `/api/inventory` | categories, items, opening-stock, stock-view |
| `reservationRoutes.js` | `/api/reservations` | create, split, lock, release |
| `dispatchRoutes.js` | `/api/dispatches` | create, loading-slip, vehicle-assign, partial |
| `eventRoutes.js` | `/api/events` | site-receipt, setup, completion, timeline |
| `returnRoutes.js` | `/api/returns` | packing-checklist, partial-return |
| `verificationRoutes.js` | `/api/verifications` | verify, damage-report, categorize |
| `repairRoutes.js` | `/api/repairs` | request, approve, vendor-assign, complete |
| `scrapRoutes.js` | `/api/scraps` | request, approve, remove, sale |
| `paymentRoutes.js` | `/api/payments` | advance, vendor, final, record, bulk-allocation |
| `expenseRoutes.js` | `/api/expenses` | CRUD, event-wise, category-wise |
| `financeRoutes.js` | `/api/finance` | cashbook, bankbook, GST, P&L |
| `invoiceRoutes.js` | `/api/invoices` | generate, PDF, send |
| `purchaseRoutes.js` | `/api/purchases` | request, approve, PO, goods-receive |
| `vendorRoutes.js` | `/api/vendors` | CRUD, payment-tracking, due-alerts |
| `staffRoutes.js` | `/api/staff` | CRUD, attendance, work-assignment |
| `vehicleRoutes.js` | `/api/vehicles` | CRUD, assign-to-dispatch |
| `reportRoutes.js` | `/api/reports` | 16 report types |
| `notificationRoutes.js` | `/api/notifications` | list, read, mark-all-read |
| `settingsRoutes.js` | `/api/settings` | company, preferences, users, roles |
| `auditRoutes.js` | `/api/audit` | logs, filter-by-user, filter-by-module |
| `aiRoutes.js` | `/api/ai` | theme-suggest, budget-calc |
| `roomRoutes.js` | `/api/rooms` | CRUD, booking, availability |
| `chatRoutes.js` | `/api/chat` | conversations, messages, media-upload, unread-count |

## Frontend Service Files (lib/services/)

Each backend route module has a corresponding frontend service file:

```
lib/services/
├── auth.services.ts          # Login, logout, token refresh
├── dashboard.services.ts     # Dashboard stats & alerts
├── crm.services.ts           # Customers, leads, site-visits
├── quotation.services.ts     # Quotation CRUD & approval
├── booking.services.ts       # Booking & agreement
├── warehouse.services.ts     # Warehouse CRUD, zones, racks
├── warehouseTransfer.services.ts  # Stock transfers
├── inventory.services.ts     # Categories, items, stock
├── reservation.services.ts   # Material reservation & split
├── dispatch.services.ts      # Loading & dispatch
├── event.services.ts         # Event execution lifecycle
├── return.services.ts        # Packing & return
├── verification.services.ts  # Verification & damage
├── repair.services.ts        # Repair management
├── scrap.services.ts         # Scrap management
├── payment.services.ts       # Payment recording & bulk allocation
├── expense.services.ts       # Expense tracking
├── finance.services.ts       # Cashbook, bankbook, GST, P&L
├── invoice.services.ts       # Invoice generation
├── purchase.services.ts      # Purchase workflow
├── vendor.services.ts        # Vendor management
├── staff.services.ts         # Staff & attendance
├── vehicle.services.ts       # Vehicle management
├── report.services.ts        # All reports
├── notification.services.ts  # Notifications
├── settings.services.ts      # Settings CRUD
├── audit.services.ts         # Audit logs
├── ai.services.ts            # AI features
├── role.services.ts          # Role management
├── chat.services.ts          # One-to-One Chat & Media Sharing
└── room.services.ts          # Resort room booking
```

## Development Phases (10-Phase Modular Strategy)

### Phase 1: Core Setup, Auth & Wizard
Monorepo init, Express server, MongoDB connection, JWT Auth, Setup Wizard, RBAC middleware.

### Phase 2: Warehouse Architecture
Dynamic Warehouses CRUD (support for adding/managing multiple godowns), Zone & Rack management, Godown stock summary.

### Phase 3: Inventory & Item Master Engine
Item Catalog, Categories, Opening Stock, Barcode generation, Inventory Ledger.

### Phase 4: CRM, Leads & Site Visits
Customer DB (Single vs Corporate Agency), Sales pipeline, Site visit scheduling.

### Phase 5: Quotations & Booking Engine
Quotation builder with real-time stock check, PDF generation, Booking & Advance payment.

### Phase 6: Material Reservation & Planning
Multi-godown stock reservation, auto-split algorithm, stock locking.

### Phase 7: Loading, Dispatch & Godown Transfer
Loading slips, Vehicle assignment, Dispatch tracking, Inter-godown transfers.

### Phase 8: Site Execution, Return & Verification
Site receipt, Setup photo uploads, Return packing checklists, Return verification (Good/Damaged/Repair/Missing/Scrap).

### Phase 9: Procurement, Vendors & HR
Purchase requests, POs, Vendor database, Staff attendance, Vehicle management.

### Phase 10: Finance, Ledgers, Reports & Audit
Advance/Final payments, Event expenses, Cashbook/Bankbook, Tax Invoices, 16 ERP Reports, Audit Logging.

## Business Department Flows (8 Departments)

### Department 1: Warehouse Management
```
Dashboard → Warehouse → Check Stock → Low Stock Alert → Purchase Request → Owner Approval → PO → Vendor → Goods Receive → Quality Check → Warehouse Entry → Inventory Update → Available Stock
```

### Department 2: New Item Add
```
Owner → Inventory → Add Item (Category, Name, Code, Unit, Purchase Cost, Rental Cost, Min Stock) → Save → Warehouse Select → Opening Stock → Ready (item appears in quotation)
```

### Department 3: Warehouse Transfer
```
Owner → Transfer Stock (From Warehouse → To Warehouse, Item, Qty) → Approval → Loading → Dispatch → Receive → Stock Updated (both warehouses)
```

### Department 4: Repair Management
```
Warehouse → Broken Item → Store Manager → Repair Request → Owner Approve → Repair Vendor → Repair Complete → Warehouse Entry → Available Again
```

### Department 5: Scrap
```
Repair Not Possible → Scrap Request → Owner Approval → Inventory Remove → Scrap Sale Entry → Finance Entry
```

### Department 6: Purchase Management
```
Vendor → Purchase Order → Goods Receive → Invoice Upload → Payment → Inventory Update → Reports
```

### Department 7: Event Order Flow (Main Revenue Flow)
```
Customer Call → CRM → Site Visit → Requirement Discussion → Event Planning → Stock Availability Check → Quotation → Customer Approval → Booking → Advance Payment
↓ (Store Team)
Booking Confirm → Reservation (multi-warehouse) → Loading Slip → Checklist Load Check → Vehicle Loading → Dispatch → Site Receive → Setup → Event Complete → Packing → Return → Verification → Available
↓ (Finance)
Booking → Advance → Expenses → Vendor Payment → Final Payment → GST → Profit → Reports
```

### Department 8: Resort Room & Venue Booking Management
```
Customer Enquiry → Resort Room Selection → Room Availability Check → Room Rate / Package Calculation → Booking → Guest Check-In → Material / Decor Request → Event Execution → Guest Check-Out → Final Billing & Invoice
```

## Role-Based Daily Workflows

### Owner Daily Workflow
```
Subah Login → Dashboard → Check (Today's Events, Pending Dispatch, Stock, Payments, Staff) → Decisions (Approve Purchases, Transfer, Quotations, Payments) → Evening Review (Dispatch Complete, Returns, Income, Expense, Profit) → Logout
```

### Store Manager Daily Workflow
```
Login → Warehouse Dashboard → Receive Purchase → Update Stock → Reserve Material → Loading → Dispatch → Receive Return → Verification → Repair → Stock Close
```

### Supervisor Workflow
```
Receive Task → Reach Site → Receive Material → Setup → Photo Upload → Attendance → Packing → Return → Close Event
```

### Accountant Workflow
```
Booking Payment → Vendor Payment → Expense Entry → Cash Book → Bank Book → GST → Invoice → Profit & Loss
```

## Coding Rules

### General
- Use TypeScript for all new frontend code.
- Use ES6+ JavaScript (or TypeScript) for backend.
- Every file must have a clear, single responsibility.
- Keep functions under 50 lines. Split large functions.
- Use meaningful variable and function names in English.
- Add JSDoc comments for all API endpoints and service functions.
- Never hardcode values. Use environment variables or constants.

### Frontend Rules
- Use Next.js App Router (`app/` directory), NOT Pages Router.
- `app/` contains ONLY `page.tsx` and `layout.tsx` — NO component logic.
- ALL UI components live in `components/dashboard/*-group/` with sub-folders.
- Every page must be 100% responsive across Mobile (<640px), Tablet (640-1024px), Laptop (1024-1536px), and Ultra-Wide Full Screen (>1536px) with full w-full max-w-full edge-to-edge fluid layout for Dashboard & Auth views.
- Use shadcn/ui components as base. Customize with Tailwind.
- Colors: Use a professional dark theme with accent colors. Avoid generic red/blue/green.
- Typography: Use Inter or Outfit font from Google Fonts.
- Animations: Use Framer Motion for page transitions and micro-interactions.
- Data fetching: Use SWR hooks for client-side fetching.
- Forms: Use React Hook Form + Zod for validation.
- State: Use React Context for auth/theme. SWR for server state.
- All interactive elements must have unique IDs.
- Loading states and error states on every page.
- Toast notifications for success/error feedback.
- Use `proxy.ts` for API proxying (Next.js 16 pattern).
- **Vercel React Best Practices**: Focus on eliminating waterfalls, optimizing server-side data fetching, and maintaining small bundle sizes.
- **Web Design & Accessibility Guidelines**: Ensure strict accessibility (ARIA labels, focus states, semantic HTML), handle forms robustly, support `prefers-reduced-motion` for animations, and implement proper UI typography.
- **Composition Patterns**: Avoid boolean prop drilling. Use compound components, state lifting, and internal composition to build flexible and scalable React APIs.
- **React View Transitions**: Implement smooth, native-feeling page transitions and route animations (like scale, slide, fade) utilizing modern Web Animations APIs or Next.js App Router integrations for a premium UI feel.

### Backend Rules
- Follow MVC pattern: Route → Controller → Service → Model.
- All routes must be protected with auth middleware (except login).
- RBAC middleware checks permissions before controller logic.
- Input validation using Joi or Zod before processing.
- All errors must be caught by global error handler.
- Every stock movement creates an Inventory Ledger entry.
- No negative stock allowed — validate before any deduction.
- Reserved stock cannot be booked again — check before reservation.
- Every dispatch must belong to an Event AND a Warehouse.
- Partial dispatch and partial return are supported.
- Damage, repair, and missing stock require remarks.
- Warehouse transfer updates both source AND destination stock.
- **Document Generation (Anthropic Patterns)**: For generating PDFs (Quotations, Invoices) and Excel files (Reports), use a structured, modular pattern separating data fetching from rendering. Utilize robust, modern libraries (e.g., `pdfkit`, `exceljs`) that can handle streaming large datasets safely in a production environment.
- All API responses follow consistent format:
  ```json
  {
    "success": true/false,
    "data": {},
    "message": "string",
    "error": null
  }
  ```

### Database Rules
- Use Mongoose for all MongoDB operations.
- Every collection must have `createdAt`, `updatedAt`, `createdBy` fields.
- Use ObjectId references for relationships (populate when needed).
- Index frequently queried fields.
- Soft delete (isDeleted flag) instead of hard delete for important records.
- Audit tables log every create/update/delete with before/after values.

## Business Rules (Critical)

1. **No Negative Stock** — System must prevent stock going below zero.
2. **Reserved = Locked** — Once reserved for an event, that stock cannot be booked for another event.
3. **Every Movement = Ledger Entry** — Purchase, transfer, reservation, dispatch, return, damage, repair, scrap — all create ledger entries.
4. **Dispatch = Event + Warehouse** — Every dispatch record belongs to a specific event and originates from a specific warehouse.
5. **Partial Operations** — Partial dispatch and partial return are fully supported.
6. **Damage/Missing = Remarks + Approval** — Cannot mark items as damaged or missing without remarks and owner approval.
7. **Transfer = Dual Update** — Warehouse transfer updates both source (deduct) and destination (add) atomically.
8. **Multi-Warehouse Planning** — When material is needed, system checks ALL warehouses and suggests optimal split.
9. **Approval Hierarchy** — Purchase, transfer, scrap, and large payments need owner approval.
10. **Single vs Corporate Customer Handling** — Retail customers have individual event ledgers. Corporate agencies have parent accounts, credit limits, 30-day net payment terms, B2B discount tiers, and bulk payment allocation across multiple invoices.
11. **Credit Limit Breach Check** — If active bookings + new booking exceeds Corporate Credit Limit, system triggers Owner Approval alert.

## User Roles & Default Permissions

| Role | Dashboard | CRM | Quotation | Booking | Warehouse | Inventory | Dispatch | Finance | Reports | Settings |
|------|-----------|-----|-----------|---------|-----------|-----------|----------|---------|---------|----------|
| Owner | ✅ Full | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Admin | ✅ Full | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Manager | ✅ Limited | ✅ | ✅ | ✅ | ✅ View | ✅ View | ✅ | ❌ | ✅ Limited | ❌ |
| Store Manager | ✅ Warehouse | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ❌ | ✅ Warehouse | ❌ |
| Accountant | ✅ Finance | ❌ | ❌ | ✅ View | ❌ | ❌ | ❌ | ✅ | ✅ Finance | ❌ |
| Supervisor | ✅ Events | ❌ | ❌ | ✅ View | ❌ | ❌ | ✅ Site | ❌ | ❌ | ❌ |
| Driver | ✅ Dispatch | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ Delivery | ❌ | ❌ | ❌ |
| Staff | ✅ Tasks | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

## Design System (Based on Master Design Blueprint in `project_images/`)

- **Theme & Aesthetics:** Modern Luxury Warm Minimalist Theme (Tailored for Event & Tent Business)
- **Primary Brand Accent / Logo Gold:** Amber/Gold (#D97706 / #F59E0B)
- **Primary Action Buttons:** Warm Chestnut Brown (#5C3A21 / #6B4627)
- **Sidebar Background:** Dark Charcoal / Zinc 900 (#18181B) with Golden Active Pill (#3F2D1F / #FBBF24)
- **Canvas & Card Backgrounds:** Light warm grey (#F8F9FA) canvas, Pure White (#FFFFFF) cards
- **Card Borders & Radius:** 1px solid #F1F5F9 border, 16px border-radius (`rounded-2xl`), subtle shadow (`shadow-sm`)
- **Status Badges Palette:**
  - Emerald Green (#10B981): Available, Confirmed, Completed, Online, Profit
  - Royal Blue (#3B82F6): Reserved, In-Transit, In Progress
  - Amber / Gold (#F59E0B): Pending Payments, Repair, Low Stock Alert
  - Red / Coral (#EF4444): Damaged, Expenses, Missing, Unpaid
  - Violet / Purple (#8B5CF6): Staff Present
- **Typography:** Inter (Headings & UI labels), Outfit (Numerals & Metric Counters)
- **Animations:** Smooth 200-300ms transitions, card hover elevation, interactive micro-animations

## Master Documentation Reference Index

For full architectural details, refer to the following authoritative documents in `project_guide/`:
- [Krishna_Tent_Events_ERP_SRS_Planning.txt](file:///d:/Artifact%20Geeks/githubworkspace/Krishna%20Tent%20&%20Events%20ERP/project_guide/Krishna_Tent_Events_ERP_SRS_Planning.txt) — Master SRS Specification (Version 2.0)
- [UI_DESIGN_SYSTEM.md](file:///d:/Artifact%20Geeks/githubworkspace/Krishna%20Tent%20&%20Events%20ERP/project_guide/UI_DESIGN_SYSTEM.md) — Master UI Theme, Palette Tokens & Dashboard Component Specifications
- [TEN_PHASE_IMPLEMENTATION_PLAN.md](file:///d:/Artifact%20Geeks/githubworkspace/Krishna%20Tent%20&%20Events%20ERP/project_guide/TEN_PHASE_IMPLEMENTATION_PLAN.md) — Detailed 10-Phase API & Component Breakdown
- [DETAILED_BUSINESS_WORKFLOW.txt](file:///d:/Artifact%20Geeks/githubworkspace/Krishna%20Tent%20&%20Events%20ERP/project_guide/DETAILED_BUSINESS_WORKFLOW.txt) — Comprehensive Narrative & Visual Workflow for 8 Departments
- [END_TO_END_WORKFLOW.txt](file:///d:/Artifact%20Geeks/githubworkspace/Krishna%20Tent%20&%20Events%20ERP/project_guide/END_TO_END_WORKFLOW.txt) — Complete Form Fields & RESTful Endpoint Specification
- [PROJECT_ESTIMATION.txt](file:///d:/Artifact%20Geeks/githubworkspace/Krishna%20Tent%20&%20Events%20ERP/project_guide/PROJECT_ESTIMATION.txt) — Project Estimation (646 Total Hours / 81 Days)
- [FRONTEND_FOLDER_STRUCTURE.md](file:///d:/Artifact%20Geeks/githubworkspace/Krishna%20Tent%20&%20Events%20ERP/project_guide/FRONTEND_FOLDER_STRUCTURE.md) — Next.js 16 Component Tree Architecture
- [BACKEND_FOLDER_STRUCTURE.md](file:///d:/Artifact%20Geeks/githubworkspace/Krishna%20Tent%20&%20Events%20ERP/project_guide/BACKEND_FOLDER_STRUCTURE.md) — Express.js Centralized `src/` Directory Structure

## Important Notes

- This is a SINGLE OWNER ERP. No multi-company, no subscription.
- The 4 warehouses are the backbone of the entire system.
- Material movement tracking is the MOST CRITICAL feature.
- Owner's daily workflow (morning check → decisions → evening review) drives the dashboard design.
- Always maintain existing comments and documentation.
- DO NOT modify files in the `project_guide/` folder.
- Run `npm run dev` for local development, NOT production build.

## Anthropic Skills Reference

> **Note:** This repository contains Anthropic's implementation of skills for Claude. For information about the Agent Skills standard, see [agentskills.io](http://agentskills.io).

[![skills.sh](https://skills.sh/b/anthropics/skills)](https://skills.sh/anthropics/skills)

# Skills
Skills are folders of instructions, scripts, and resources that Claude loads dynamically to improve performance on specialized tasks. Skills teach Claude how to complete specific tasks in a repeatable way, whether that's creating documents with your company's brand guidelines, analyzing data using your organization's specific workflows, or automating personal tasks.

For more information, check out:
- [What are skills?](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [How to create custom skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)
- [Equipping agents for the real world with Agent Skills](https://anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)

# About This Repository

This repository contains skills that demonstrate what's possible with Claude's skills system. These skills range from creative applications (art, music, design) to technical tasks (testing web apps, MCP server generation) to enterprise workflows (communications, branding, etc.).

Each skill is self-contained in its own folder with a `SKILL.md` file containing the instructions and metadata that Claude uses. Browse through these skills to get inspiration for your own skills or to understand different patterns and approaches.

Many skills in this repo are open source (Apache 2.0). We've also included the document creation & editing skills that power [Claude's document capabilities](https://www.anthropic.com/news/create-files) under the hood in the [`skills/docx`](./skills/docx), [`skills/pdf`](./skills/pdf), [`skills/pptx`](./skills/pptx), and [`skills/xlsx`](./skills/xlsx) subfolders. These are source-available, not open source, but we wanted to share these with developers as a reference for more complex skills that are actively used in a production AI application.

## Disclaimer

**These skills are provided for demonstration an
