# Krishna Tent & Events ERP - 10-Phase Implementation Plan

## Overview & Strategy
Given the comprehensive nature of this **Warehouse-Centric Event ERP**, the development and testing process is divided into **10 modular, incremental phases**. Each phase focuses on building and fully testing a specific slice of the system (Backend REST APIs + Frontend UI Components + Service Integrations) to ensure zero bugs before moving to the next phase.

---

## Phase Summary Table

| Phase | Phase Name | Core Focus & Deliverables | Testing & Verification |
|---|---|---|---|
| **Phase 1** | Monorepo Setup, Core Infra & Auth | Project init, MongoDB, Express Server, JWT Auth, Setup Wizard, RBAC Middleware | Postman Auth API tests, JWT validation, Setup Wizard UI flow |
| **Phase 2** | Warehouse Architecture & Multi-Godown Management | 4 Warehouses CRUD, Zones & Racks management, Warehouse Stock Dashboard | Warehouse API unit tests, Zone/Rack nesting tests |
| **Phase 3** | Inventory & Item Master Engine | Categories, Items CRUD, SKU/Code setup, Multi-Warehouse Opening Stock, Inventory Ledger | Stock addition API tests, Ledger entry generation tests |
| **Phase 4** | CRM, Leads & Site Visit Workflow | Customer database, Lead status tracking, Site visit scheduling | Lead pipeline transition tests, Site visit booking tests |
| **Phase 5** | Quotation Builder & Booking Agreement | Quotation engine with live stock check, PDF generation, Booking & Advance payment | Multi-warehouse stock availability check, PDF export validation |
| **Phase 6** | Stock Reservation & Planning Engine | Auto multi-warehouse stock reservation, split allocation, locked stock logic | Stock lock validation, split algorithm verification |
| **Phase 7** | Loading, Dispatch & Inter-Warehouse Transfers | Loading slips, Vehicle assignment, Dispatch tracking, Warehouse transfer workflow | Dispatch status updates, Dual-warehouse transfer stock calculation |
| **Phase 8** | Site Execution, Returns & Verification | Site receipt confirmation, Photo upload, Return packing checklist, Item verification (Good/Damaged/Repair/Missing/Scrap) | Partial return tests, Damage classification & Repair request flow |
| **Phase 9** | Procurement, Vendors & Staff Management | Purchase Requests, POs, Vendor database, Staff database, Attendance tracking | PO lifecycle tests, Attendance calculation & vehicle assignment tests |
| **Phase 10** | Finance, Ledgers, Reports & Audit Engine | Cashbook, Bankbook, GST Invoices, Event P&L, 16 Comprehensive Reports, Audit Logging | Profit calculation accuracy, Report generation, System audit log verification |

---

## Detailed Phase Specifications

---

### 🔹 PHASE 1: Monorepo Setup, Core Infra & Authentication
**Goal:** Establish workspace environment, database schemas for users/roles, JWT authentication, RBAC middleware, and setup wizard.

#### Backend REST APIs & Models (`backend/`):
- **Models:** `User.js`, `Role.js`, `Company.js`, `AuditLog.js`
- **Routes & Endpoints (`auth.routes.js`, `settings.routes.js`):**
  - `POST /api/auth/login` — Authenticate user, issue JWT & refresh token
  - `POST /api/auth/logout` — Clear token / invalidate session
  - `POST /api/auth/refresh` — Issue new access token using refresh token
  - `GET /api/auth/me` — Fetch current logged-in user profile & permissions
  - `POST /api/settings/setup-wizard` — First-time company profile setup
- **Middleware:** `authMiddleware.js`, `rbacMiddleware.js`, `errorHandler.js`

#### Frontend Routes & Components (`frontend/src/`):
- **App Routes:** `app/(auth)/login/page.tsx`, `app/(auth)/setup-wizard/page.tsx`
- **Components:** `components/auth/LoginForm.tsx`, `components/auth/SetupWizardForm.tsx`, `components/auth/AuthGuard.tsx`
- **Services:** `lib/services/auth.services.ts`, `lib/services/settings.services.ts`
- **State/Context:** `contexts/AuthContext.tsx`

#### Phase 1 Testing Checklist:
1. [ ] Test invalid login credentials return HTTP 401.
2. [ ] Test successful login receives JWT and populates AuthContext.
3. [ ] Test RBAC middleware blocks non-permission routes.
4. [ ] Test first-time Setup Wizard creates owner account and company profile.

---

### 🔹 PHASE 2: Warehouse Architecture & Multi-Godown Management
**Goal:** Build the backbone godown infrastructure for 4 Warehouses, Zones, and Racks.

#### Backend REST APIs & Models (`backend/`):
- **Models:** `Warehouse.js`, `Zone.js`, `Rack.js`
- **Routes & Endpoints (`warehouse.routes.js`):**
  - `GET /api/warehouses` — List all 4 warehouses with stock summary
  - `POST /api/warehouses` — Create new warehouse
  - `GET /api/warehouses/:id` — Get warehouse details with zones/racks
  - `PUT /api/warehouses/:id` — Update warehouse info
  - `POST /api/warehouses/:id/zones` — Add zone to warehouse
  - `POST /api/warehouses/:id/racks` — Add rack to zone

#### Frontend Routes & Components (`frontend/src/`):
- **App Routes:** `app/(dashboard)/warehouses/page.tsx`, `app/(dashboard)/warehouses/new/page.tsx`, `app/(dashboard)/warehouses/[id]/page.tsx`
- **Components:** `components/dashboard/warehouse-group/warehouses/WarehousesView.tsx`, `WarehouseDetailView.tsx`, `WarehouseForm.tsx`, `ZoneManager.tsx`, `RackManager.tsx`
- **Services:** `lib/services/warehouse.services.ts`

#### Phase 2 Testing Checklist:
1. [ ] Test CRUD operations on Warehouses.
2. [ ] Test nested creation of Zones within Warehouses and Racks within Zones.
3. [ ] Verify stock summary per warehouse displays correct counts.

---

### 🔹 PHASE 3: Inventory & Item Master Engine
**Goal:** Catalog items with rental rates, purchase costs, min stock alerts, unique SKU/Codes, and opening stock.

#### Backend REST APIs & Models (`backend/`):
- **Models:** `Category.js`, `Item.js`, `InventoryLedger.js`, `WarehouseStock.js`
- **Routes & Endpoints (`inventory.routes.js`):**
  - `GET /api/inventory/categories` — List item categories
  - `POST /api/inventory/categories` — Add new category
  - `GET /api/inventory/items` — Paginated item list with filters
  - `POST /api/inventory/items` — Create new rental/sale item
  - `POST /api/inventory/items/:id/opening-stock` — Assign opening stock per warehouse
  - `GET /api/inventory/ledger` — View item movement ledger

#### Frontend Routes & Components (`frontend/src/`):
- **App Routes:** `app/(dashboard)/inventory/page.tsx`, `app/(dashboard)/inventory/categories/page.tsx`, `app/(dashboard)/inventory/items/new/page.tsx`, `app/(dashboard)/inventory/ledger/page.tsx`
- **Components:** `components/dashboard/inventory-group/items/ItemsView.tsx`, `ItemForm.tsx`, `ItemDetailView.tsx`, `components/dashboard/inventory-group/categories/CategoriesView.tsx`, `components/dashboard/inventory-group/ledger/LedgerView.tsx`
- **Services:** `lib/services/inventory.services.ts`

#### Phase 3 Testing Checklist:
1. [ ] Test item creation with unique SKU code.
2. [ ] Test opening stock entry creates initial `InventoryLedger` record.
3. [ ] Verify low-stock warning triggers when available stock < minimum stock.

---

### 🔹 PHASE 4: CRM, Leads & Site Visit Workflow
**Goal:** Capture customer enquiries, schedule site visits, and track lead conversion.

#### Backend REST APIs & Models (`backend/`):
- **Models:** `Customer.js`, `Lead.js`, `SiteVisit.js`
- **Routes & Endpoints (`crm.routes.js`):**
  - `GET /api/crm/customers` — List/search customers
  - `POST /api/crm/customers` — Add customer record
  - `GET /api/crm/leads` — Lead pipeline board/list
  - `POST /api/crm/leads` — Create lead entry
  - `PUT /api/crm/leads/:id/status` — Update lead status (New → Contacted → Site Visit → Quotation → Booked → Lost)
  - `POST /api/crm/site-visits` — Schedule site visit & assign staff

#### Frontend Routes & Components (`frontend/src/`):
- **App Routes:** `app/(dashboard)/crm/page.tsx`, `app/(dashboard)/crm/customers/page.tsx`, `app/(dashboard)/crm/leads/page.tsx`, `app/(dashboard)/crm/site-visits/page.tsx`
- **Components:** `components/dashboard/crm-group/customers/CustomersView.tsx`, `CustomerForm.tsx`, `components/dashboard/crm-group/leads/LeadsView.tsx`, `LeadForm.tsx`, `components/dashboard/crm-group/site-visits/SiteVisitsView.tsx`, `SiteVisitForm.tsx`
- **Services:** `lib/services/crm.services.ts`

#### Phase 4 Testing Checklist:
1. [ ] Test adding customer and auto-populating CRM details.
2. [ ] Test lead status updates across the sales pipeline.
3. [ ] Test scheduling a site visit and verifying staff assignment.

---

### 🔹 PHASE 5: Quotation Builder & Booking Agreement
**Goal:** Build event quotations with real-time multi-warehouse availability check, generate PDFs, and convert to confirmed bookings.

#### Backend REST APIs & Models (`backend/`):
- **Models:** `Quotation.js`, `Booking.js`, `Agreement.js`
- **Routes & Endpoints (`quotation.routes.js`, `booking.routes.js`):**
  - `POST /api/quotations` — Create quotation with selected inventory items
  - `POST /api/quotations/check-availability` — Check item availability across 4 warehouses for event date range
  - `GET /api/quotations/:id/pdf` — Generate quotation PDF document
  - `POST /api/bookings/convert-quotation/:quotationId` — Convert approved quotation into event booking
  - `POST /api/bookings/:id/agreement` — Generate rental agreement contract

#### Frontend Routes & Components (`frontend/src/`):
- **App Routes:** `app/(dashboard)/quotations/page.tsx`, `app/(dashboard)/quotations/new/page.tsx`, `app/(dashboard)/bookings/page.tsx`, `app/(dashboard)/bookings/[id]/page.tsx`
- **Components:** `components/dashboard/quotation-group/QuotationsView.tsx`, `QuotationForm.tsx`, `ItemSelector.tsx`, `StockAvailabilityCheck.tsx`, `components/dashboard/booking-group/BookingsView.tsx`, `BookingForm.tsx`, `AgreementView.tsx`
- **Services:** `lib/services/quotation.services.ts`, `lib/services/booking.services.ts`

#### Phase 5 Testing Checklist:
1. [ ] Test stock availability check correctly factors in existing bookings for the target dates.
2. [ ] Test PDF generation for quotations and agreements.
3. [ ] Test quotation conversion to booking locks quotation status.

---

### 🔹 PHASE 6: Stock Reservation & Planning Engine
**Goal:** Lock material for confirmed events and provide intelligent reservation split suggestions across 4 warehouses.

#### Backend REST APIs & Models (`backend/`):
- **Models:** `Reservation.js`, `StockLock.js`
- **Routes & Endpoints (`reservation.routes.js`):**
  - `POST /api/reservations` — Create material reservation for booking
  - `POST /api/reservations/suggest-split` — AI/Algorithm suggestion for optimal stock allocation from Godown A, B, C, D
  - `POST /api/reservations/:id/lock` — Lock reserved quantities in inventory
  - `POST /api/reservations/:id/release` — Cancel/Release locked stock

#### Frontend Routes & Components (`frontend/src/`):
- **App Routes:** `app/(dashboard)/reservation/page.tsx`, `app/(dashboard)/reservation/[bookingId]/page.tsx`
- **Components:** `components/dashboard/operations-group/reservation/ReservationView.tsx`, `ReservationForm.tsx`, `WarehouseAvailability.tsx`, `ReservationSplit.tsx`
- **Services:** `lib/services/reservation.services.ts`

#### Phase 6 Testing Checklist:
1. [ ] Test auto-split algorithm allocates stock based on proximity and warehouse balance.
2. [ ] Verify reserved stock cannot be allocated to another overlapping booking.
3. [ ] Test stock lock updates item state to `Reserved`.

---

### 🔹 PHASE 7: Loading, Dispatch & Inter-Warehouse Transfers
**Goal:** Generate godown loading slips, assign drivers/vehicles, dispatch material, and execute inter-warehouse transfers.

#### Backend REST APIs & Models (`backend/`):
- **Models:** `Dispatch.js`, `LoadingSlip.js`, `WarehouseTransfer.js`
- **Routes & Endpoints (`dispatch.routes.js`, `warehouseTransfer.routes.js`):**
  - `POST /api/dispatches` — Create dispatch order for event
  - `GET /api/dispatches/:id/loading-slip` — Generate loading checklist per godown
  - `PUT /api/dispatches/:id/status` — Mark status (Loading → Dispatched → In-Transit → Delivered)
  - `POST /api/warehouse-transfers` — Initiate inter-godown stock transfer
  - `PUT /api/warehouse-transfers/:id/receive` — Confirm receipt at destination godown

#### Frontend Routes & Components (`frontend/src/`):
- **App Routes:** `app/(dashboard)/dispatches/page.tsx`, `app/(dashboard)/dispatches/new/page.tsx`, `app/(dashboard)/warehouses/transfer/page.tsx`
- **Components:** `components/dashboard/operations-group/dispatches/DispatchesView.tsx`, `DispatchForm.tsx`, `LoadingChecklist.tsx`, `components/dashboard/warehouse-group/transfer/StockTransfersView.tsx`, `StockTransferForm.tsx`
- **Services:** `lib/services/dispatch.services.ts`, `lib/services/warehouseTransfer.services.ts`

#### Phase 7 Testing Checklist:
1. [ ] Test loading slip generation per warehouse.
2. [ ] Test dispatch execution updates stock state from `Reserved` to `Dispatched/At Site`.
3. [ ] Test inter-warehouse transfer deducts source godown and adds to destination godown atomically.

---

### 🔹 PHASE 8: Site Execution, Returns & Verification
**Goal:** Manage on-site receipt, event setup photos, return packing checklists, godown return verification, and damage/repair classification.

#### Backend REST APIs & Models (`backend/`):
- **Models:** `EventExecution.js`, `Return.js`, `Verification.js`, `Repair.js`, `Scrap.js`
- **Routes & Endpoints (`event.routes.js`, `return.routes.js`, `verification.routes.js`, `repair.routes.js`, `scrap.routes.js`):**
  - `POST /api/events/:id/site-receipt` — Confirm site receipt & upload photos
  - `POST /api/returns` — Create return loading slip
  - `POST /api/verifications` — Verify returned stock at godown (Classify: Good, Damaged, Repair, Missing, Scrap)
  - `POST /api/repairs` — Create repair request for damaged items
  - `POST /api/scraps` — Create scrap request for unrepairable items

#### Frontend Routes & Components (`frontend/src/`):
- **App Routes:** `app/(dashboard)/events/page.tsx`, `app/(dashboard)/events/[id]/site-receipt/page.tsx`, `app/(dashboard)/events/[id]/return/page.tsx`, `app/(dashboard)/events/[id]/verification/page.tsx`
- **Components:** `components/dashboard/operations-group/events/EventsView.tsx`, `EventDetailView.tsx`, `SiteReceiptForm.tsx`, `PhotoUpload.tsx`, `PackingChecklist.tsx`, `ReturnForm.tsx`, `VerificationForm.tsx`, `DamageReport.tsx`
- **Services:** `lib/services/event.services.ts`, `lib/services/return.services.ts`, `lib/services/verification.services.ts`, `lib/services/repair.services.ts`, `lib/services/scrap.services.ts`

#### Phase 8 Testing Checklist:
1. [ ] Test photo upload for site setup using Cloudinary integration.
2. [ ] Test return verification classifying 10 chairs as Good and 2 as Damaged.
3. [ ] Verify Good items return to `Available` stock while Damaged items move to `Repair`.

---

### 🔹 PHASE 9: Procurement, Vendors & Staff Management
**Goal:** Handle stock replenishment purchase orders, vendor balances, staff attendance, work assignments, and vehicle dispatch allocation.

#### Backend REST APIs & Models (`backend/`):
- **Models:** `PurchaseOrder.js`, `Vendor.js`, `Staff.js`, `Attendance.js`, `Vehicle.js`, `ChatConversation.js`, `ChatMessage.js`
- **Routes & Endpoints (`purchase.routes.js`, `vendor.routes.js`, `staff.routes.js`, `vehicle.routes.js`, `chat.routes.js`):**
  - `POST /api/purchases` — Create Purchase Request / PO
  - `PUT /api/purchases/:id/approve` — Owner approval for PO
  - `POST /api/purchases/:id/receive` — Receive goods & auto-update inventory
  - `GET /api/vendors` — Vendor list & payment tracking
  - `POST /api/staff/attendance` — Log daily/event staff attendance
  - `GET /api/vehicles` — Vehicle fleet list & availability
  - `GET /api/chat/conversations` — Fetch user direct conversations
  - `POST /api/chat/messages` — Send 1-on-1 text, media attachment, or voice note message
  - `POST /api/chat/upload` — Upload voice note or file attachment via Cloudinary

#### Frontend Routes & Components (`frontend/src/`):
- **App Routes:** `app/(dashboard)/purchases/page.tsx`, `app/(dashboard)/vendors/page.tsx`, `app/(dashboard)/staff/page.tsx`, `app/(dashboard)/staff/attendance/page.tsx`, `app/(dashboard)/vehicles/page.tsx`, `app/(dashboard)/chat/page.tsx`, `app/(dashboard)/messages/page.tsx`
- **Components:** `components/dashboard/purchases-group/purchases/PurchasesView.tsx`, `PurchaseForm.tsx`, `components/dashboard/purchases-group/vendors/VendorsView.tsx`, `VendorForm.tsx`, `components/dashboard/hr-group/staff/StaffView.tsx`, `AttendanceView.tsx`, `components/dashboard/hr-group/vehicles/VehiclesView.tsx`, `components/dashboard/chat-group/ChatView.tsx`, `MessagesView.tsx`, `ChatConversationList.tsx`, `ChatMessageWindow.tsx`, `ChatUserDirectory.tsx`, `MediaAttachmentModal.tsx`, `VoiceRecorder.tsx`, `AudioPlayerBubble.tsx`
- **Services & Providers:** `lib/services/purchase.services.ts`, `lib/services/vendor.services.ts`, `lib/services/staff.services.ts`, `lib/services/vehicle.services.ts`, `lib/services/chat.services.ts`, `provider/ChatProvider.tsx`, `hooks/useChatSocket.ts`, `store/chatSlice.ts`

#### Phase 9 Testing Checklist:
1. [ ] Test purchase order approval workflow.
2. [ ] Test goods receipt automatically increases warehouse stock and logs vendor ledger.
3. [ ] Test staff attendance logging and vehicle assignment to dispatches.
4. [ ] Test real-time 1-on-1 team chat, automated team invite welcome messages, and WhatsApp-style voice note recording & audio playback.

---

### 🔹 PHASE 10: Finance, Ledgers, Reports & Audit Engine
**Goal:** Complete event financial tracking, cashbook/bankbook, GST invoicing, 16 business reports, audit trail, and dashboard analytics.

#### Backend REST APIs & Models (`backend/`):
- **Models:** `Payment.js`, `Expense.js`, `Invoice.js`, `Cashbook.js`, `Bankbook.js`, `Notification.js`
- **Routes & Endpoints (`payment.routes.js`, `expense.routes.js`, `finance.routes.js`, `invoice.routes.js`, `reports.routes.js`, `dashboard.routes.js`, `notification.routes.js`, `audit.routes.js`):**
  - `POST /api/payments` — Record booking advance/final payment or vendor payment
  - `POST /api/expenses` — Log event-wise or operational expense
  - `GET /api/finance/cashbook` — Cashbook entries & balance
  - `GET /api/finance/bankbook` — Bankbook entries & reconciliation
  - `POST /api/invoices/generate` — Generate tax invoice with GST
  - `GET /api/reports/:reportType` — Generate 16 report types (Sales, Profit, Inventory, Damage, Transfer, etc.)
  - `GET /api/dashboard/stats` — Owner dashboard metrics & alerts
  - `GET /api/audit` — Audit trail log viewer

#### Frontend Routes & Components (`frontend/src/`):
- **App Routes:** `app/(dashboard)/page.tsx`, `app/(dashboard)/finance/payments/page.tsx`, `app/(dashboard)/finance/expenses/page.tsx`, `app/(dashboard)/finance/invoices/page.tsx`, `app/(dashboard)/finance/cashbook/page.tsx`, `app/(dashboard)/finance/bankbook/page.tsx`, `app/(dashboard)/reports/page.tsx`, `app/(dashboard)/calendar/page.tsx`, `app/(dashboard)/gallery/page.tsx`, `app/(dashboard)/notifications/page.tsx`
- **Components:** `components/dashboard/dashboard/DashboardView.tsx`, `CalendarWidget.tsx`, `components/dashboard/finance-group/payments/PaymentsView.tsx`, `components/dashboard/finance-group/expenses/ExpensesView.tsx`, `components/dashboard/finance-group/invoices/InvoicesView.tsx`, `InvoiceBuilder.tsx`, `components/dashboard/finance-group/cashbook/CashbookView.tsx`, `components/dashboard/finance-group/bankbook/BankbookView.tsx`, `components/dashboard/reports/` (All report views), `components/dashboard/calendar/EventCalendar.tsx`
- **Services:** `lib/services/payment.services.ts`, `lib/services/expense.services.ts`, `lib/services/finance.services.ts`, `lib/services/invoice.services.ts`, `lib/services/report.services.ts`, `lib/services/dashboard.services.ts`, `lib/services/notification.services.ts`, `lib/services/audit.services.ts`

#### Phase 10 Testing Checklist:
1. [ ] Test accurate Event P&L calculation: Revenue (Booking) - Expenses - Vendor Payments = Net Profit.
2. [ ] Test GST calculation and tax invoice PDF export.
3. [ ] Test Cashbook and Bankbook auto-posting from payments/expenses.
4. [ ] Verify all 16 reports render accurate historical data.
5. [ ] Verify Audit Log records user actions across all 10 phases.
