# Krishna Tent & Events ERP - Master UI Design System & Component Visual Architecture (Dual Theme: Light & Dark Mode)

## Overview
This document defines the complete visual design system, dual-theme tokens (Light Mode & Dark Mode), typography, component styling, layout grid, and UI view mappings for **Krishna Tent & Events ERP**.

The design system supports **seamless switching between Light Mode & Dark Mode** via `ThemeProvider.tsx` and a Theme Toggle button in the Top Navigation Bar (`TopNavBar.tsx`).

The visual aesthetics are directly derived from the master design blueprint in [`project_images/Krishna Tent & Events ERP.png`](file:///d:/Artifact%20Geeks/githubworkspace/Krishna%20Tent%20&%20Events%20ERP/project_images/Krishna%20Tent%20&%20Events%20ERP.png) and 100% synchronized with [`FRONTEND_FOLDER_STRUCTURE.md`](file:///d:/Artifact%20Geeks/githubworkspace/Krishna%20Tent%20&%20Events%20ERP/project_guide/FRONTEND_FOLDER_STRUCTURE.md).

---

## 🎨 Dual Theme Palette Tokens (Light & Dark Mode)

### 1. Brand Identity Tokens (Constant Across Themes)
- **Brand Name:** Krishna Tent & Events ERP
- **Brand Logo & Accent:** `#D97706` (Amber 600) / `#F59E0B` (Amber 500)
- **Primary Action Button:** `#5C3A21` (Warm Chestnut Brown) / `#6B4627` with White Text (`#FFFFFF`)

### 2. Top Sidebar Warehouse Switcher Dropdown (`components/dashboard/layout/WarehouseSwitcher.tsx`)
Positioned prominently at the **TOP OF THE SIDEBAR** right below the Brand Header (matching the workspace switcher dropdown design):
- **Dropdown Label / Header:** `SWITCH GODOWN / WAREHOUSE`
- **Dropdown Container:** Styled rounded pill (`bg-zinc-800/80 dark:bg-zinc-900 border border-zinc-700/60 p-2.5 shadow-sm`)
- **Options Menu:**
  1. `All Warehouses (Godowns Overview)` — Default consolidated view across all dynamic godowns
  2. `Main Warehouse` (● Online) — Filters entire dashboard & inventory stats to Main Godown
  3. `Jaipur Warehouse` (● Online) — Filters entire dashboard & inventory stats to Jaipur Godown
  4. `Ajmer Warehouse` (● Low Stock Alert) — Filters entire dashboard & inventory stats to Ajmer Godown
  5. `Jodhpur Warehouse` (● Online) — Filters entire dashboard & inventory stats to Jodhpur Godown
  6. `+ Add Godown` — Quick action modal trigger for Owner/Admin
- **Interactive Behavior:**
  - Selecting any warehouse from this top dropdown instantly filters all Dashboard KPI Cards, Inventory Charts, Warehouse Tables, Dispatches, and Return Queues to show real-time data specifically for that selected Godown.
  - Selecting "All Warehouses" restores the consolidated multi-godown overview.

### 3. Sidebar Navigation Links (`components/dashboard/layout/SideNavBar.tsx`)
- **Sidebar Canvas:** `#18181B` (Dark Charcoal / Zinc 900)
- **Logo Header:** `#09090B` (Deep Black Header)
- **Normal Nav Text:** `#A1A1AA` (Zinc 400) with 14px font size & 500 weight
- **Hover State:** `#27272A` (Zinc 800) background with `#F4F4F5` text
- **Active Nav Pill:** `#3F2D1F` / `#523B27` background with `#FBBF24` (Gold) active text & glow indicator

### 4. Surface & Card Tokens
- **App Canvas Background:** `#F8F9FA` (Soft Light Grey)
- **Card Surface:** `#FFFFFF` (Pure White)
- **Card Border:** `1px solid #F1F5F9` (Slate 100)
- **Card Border Radius:** `16px` (`rounded-2xl`)
- **Card Box Shadow:** `0 1px 3px 0 rgba(0, 0, 0, 0.05)` (`shadow-sm`)
- **Card Hover Effect:** `hover:shadow-md hover:-translate-y-0.5 transition-all duration-200`

### 5. Status Badge Colors (`components/common/StatusBadge.tsx`)
- **Emerald Green (`#10B981` / `#059669`):** `Available`, `Confirmed`, `Completed`, `Online`, `Profit`, `Delivered`
- **Royal Blue (`#3B82F6` / `#2563EB`):** `Reserved`, `In-Transit`, `In Progress`, `Loading`
- **Amber Gold (`#F59E0B` / `#D97706`):** `Pending`, `Repair`, `Low Stock Alert`, `Review`
- **Coral Red (`#EF4444` / `#DC2626`):** `Damaged`, `Expenses`, `Missing`, `Unpaid`, `Lost`
- **Violet Purple (`#8B5CF6`):** `Staff Present`, `Catering`, `Special Decor`
- **Slate Grey (`#64748B`):** `Draft`, `Others`, `Cancelled`

### 6. Tailwind v4 `app/globals.css` Blueprint (Centralized CSS)
*Note: These colors are mapped from the Action Button reference image (Warm Brown, Deep Teal, Gold, Coral Red) alongside our base Zinc/Amber theme.*

```css
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@100..900&family=Outfit:wght@100..900&display=swap");
@import "tailwindcss";
@custom-variant dark (&:is(.dark *));

:root {
  /* Core Colors */
  --background: #eaeff5; /* Soft blue-grey from image */
  --foreground: #111827;
  --card: #ffffff;
  --card-foreground: #111827;
  
  /* Primary Action - Warm Chestnut Brown (Image) */
  --primary: #8a5a32;
  --on-primary: #ffffff;
  --primary-container: #e6d8c8;
  --on-primary-container: #472b14;
  
  /* Secondary Action - Deep Teal (Image) */
  --secondary: #3c646b;
  --on-secondary: #ffffff;
  --secondary-container: #c4dadd;
  --on-secondary-container: #1b3438;
  
  /* Tertiary / Accent - Gold/Mustard (Image) */
  --tertiary: #93701e;
  --on-tertiary: #ffffff;
  --tertiary-container: #f5ebd5;
  --on-tertiary-container: #4a370b;
  
  /* Destructive - Coral Red (Image) */
  --error: #ba292e;
  --on-error: #ffffff;
  --error-container: #fbd5d7;
  --on-error-container: #681215;

  /* Surfaces & Muted */
  --muted: #e4eaf1;
  --muted-foreground: #576065;
  --border: #dbe2eb;
  --input: #dbe2eb;
  --ring: #8a5a32;
  
  --radius-sm: 0.25rem;
  --radius-md: 0.75rem; /* 12px for inputs */
  --radius-lg: 1rem;    /* 16px for buttons */
  --radius-xl: 1.25rem; /* 20px for cards */
}

.dark {
  --background: #18181B; /* Zinc 900 */
  --foreground: #f8fafc;
  --card: #27272A; /* Zinc 800 */
  --card-foreground: #f8fafc;
  
  --primary: #c28854; /* Lighter brown for dark mode */
  --on-primary: #38200d;
  --primary-container: #633f21;
  --on-primary-container: #f4e3d3;
  
  --secondary: #5a8e96; /* Lighter teal */
  --on-secondary: #122c30;
  --secondary-container: #2b494f;
  --on-secondary-container: #daeef1;
  
  --tertiary: #d1a536;
  --on-tertiary: #3a2a05;
  --tertiary-container: #684f11;
  --on-tertiary-container: #fbf0cf;

  --error: #ffb4ab;
  --on-error: #690005;
  --error-container: #93000a;
  --on-error-container: #ffdad6;

  --muted: #3f3f46; /* Zinc 700 */
  --muted-foreground: #a1a1aa;
  --border: #3f3f46;
  --input: #3f3f46;
  --ring: #c28854;
}

@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-card: var(--card);
  --color-card-foreground: var(--card-foreground);
  --color-primary: var(--primary);
  --color-secondary: var(--secondary);
  --color-tertiary: var(--tertiary);
  --color-error: var(--error);
  --color-muted: var(--muted);
  --color-border: var(--border);
  --color-input: var(--input);
  --color-ring: var(--ring);

  --radius-sm: var(--radius-sm);
  --radius-md: var(--radius-md);
  --radius-lg: var(--radius-lg);
  --radius-xl: var(--radius-xl);
  
  --font-sans: 'Inter', sans-serif;
  --font-mono: 'Outfit', monospace;
}

@layer base {
  * { @apply border-border outline-ring/50; }
  body { @apply bg-background text-foreground overflow-x-hidden font-sans antialiased; }
}

/* Centralized Glassmorphism & Utilities */
.glass-card {
  background: color-mix(in srgb, var(--card) 95%, transparent);
  border: 1px solid color-mix(in srgb, var(--border) 60%, transparent);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.03);
  transition: all 0.3s ease;
}
.glass-card:hover { box-shadow: 0 8px 30px rgba(138, 90, 50, 0.08); }
.glass-input {
  background: var(--card); border: 1px solid var(--border);
  border-radius: var(--radius-md); transition: all 0.2s;
}
.glass-input:focus { border-color: var(--primary); box-shadow: 0 0 0 4px rgba(138, 90, 50, 0.15); outline: none; }
```

---

## 🏛️ UI Layout & Main Dashboard Architecture (`DashboardView.tsx`)

The Main Dashboard (`components/dashboard/dashboard/DashboardView.tsx`) is structured into 4 main grid rows matching the master design screenshot:

### Row 1: Top 7 KPI Cards Grid
1. `TODAY'S EVENTS` — Icon: Calendar, Count: 3 Events, Subtext: "View all →"
2. `TODAY'S DISPATCH` — Icon: Truck, Count: 8 Dispatches, Subtext: "View all →"
3. `TODAY'S RETURNS` — Icon: Refresh, Count: 5 Returns, Subtext: "View all →"
4. `PENDING PAYMENTS` — Icon: Wallet, Amount: ₹ 2,45,000 (5 Invoices), Subtext: "View all →"
5. `AVAILABLE STOCK` — Icon: Box, Count: 12,458 Items, Subtext: "View all →"
6. `MATERIAL AT SITE` — Icon: MapPin, Count: 4,820 Items, Subtext: "View all →"
7. `STAFF PRESENT` — Icon: Users, Percentage: 86% (32/37), Subtext: "View all →"

### Row 2: Analytics Charts & Godown Summary
- **REVENUE OVERVIEW:** Smooth curved area line chart showing monthly revenue (`₹ 18,75,000` | `+24.5% vs Last Month`)
- **PROFIT & LOSS OVERVIEW:** Dual bar chart comparing Profit (Green `#10B981`) vs Expenses (Brown `#6B4627`) (`₹ 6,45,000` | `+18.7%`)
- **WAREHOUSE SUMMARY TABLE:**
  - Columns: `Warehouse`, `Available`, `Reserved`, `At Site`, `Loading`, `Repair`, `Damaged`, `Missing`
  - Rows: [Dynamic List of Warehouses], Total Summary Row

### Row 3: Operational Cycles & Calendar
- **INVENTORY STATUS:** Donut chart breakdown (Available 34%, Reserved 37%, Damaged 7%, Repair 8%, Others 14%)
- **MATERIAL MOVEMENT FLOW:** Step-by-step visual cycle diagram:
  `Available (12,458)` → `Reserved (5,645)` → `Loading (1,245)` → `Dispatch (8,452)` → `At Site (4,820)` → `Return (1,258)` → `Verification (845)` → `Available Again (12,458)`
- **EVENT CALENDAR WIDGET:** Monthly view with event badges (Sharma Wedding, Gupta Wedding, Meena Reception)

### Row 4: Live Activity & Action Widgets
- **RECENT BOOKINGS TABLE:** Booking ID, Customer, Event Date, Location, Amount, Status Badge (`At Site`, `Confirmed`, `Loading`, `Pending`)
- **DISPATCH TIMELINE (TODAY):** Step timeline (Loaded 10:00 AM, In Transit 01:30 PM, At Site 05:10 PM, Completed 10:00 PM)
- **WEATHER ALERT CARD:** Live venue weather monitoring (Jaipur, Rajasthan 32°C Light Rain Expected)
- **AI SUGGESTIONS CARD (DeepSeek Integration):** Smart alerts for low stock, pending payments, weather risk, repair reminders
- **PENDING DISPATCH & RETURNS TABLES:** Queue tracking for dispatches and return trucks
- **UPCOMING TASKS CHECKLIST:** Quick interactive task checklist for godown team

---

## 🧩 Component Group Visual Specifications (Mapping FRONTEND_FOLDER_STRUCTURE.md)

### 1. Common UI Components (`components/common/`)
- `Button.tsx`: Variants for `primary` (Warm Brown `#5C3A21`), `secondary` (White/Dark border), `danger` (Red), `ghost`. Sizes: `sm`, `md`, `lg`.
- `Card.tsx`: Standard card container with `rounded-2xl`, dual-theme surface (`bg-white dark:bg-zinc-900`), `p-6` padding, border (`border-slate-100 dark:border-zinc-800`).
- `DataTable.tsx`: Dual-theme styled table component with sortable headers, search filter, pagination controls, hoverable rows, and custom status badge renderers.
- `PageHeader.tsx`: Page Title, Subtitle breadcrumb, Primary Action Button (e.g. `+ Add Customer`).
- `StatsCard.tsx`: Reusable metric card with icon badge, title, big stat numeral, and subtext link.
- `StatusBadge.tsx`: Pill badge with dot indicator and tailored status background/text color tokens.
- `FormDrawer.tsx`: Slide-in right drawer for quick creation/edit forms without leaving the current view.
- `ConfirmModal.tsx` & `DeleteModal.tsx`: Styled confirmation dialogs for destructive actions.
- `Skeleton.tsx`, `DetailViewSkeleton.tsx`, `ViewPageSkeleton.tsx`: Skeleton loading states.

### 2. CRM Group Components (`components/dashboard/crm-group/`)
- **Customers (`crm-group/customers/`):**
  - `CustomersView.tsx`: Customer table listing type (Retail vs Corporate Agency), phone, total bookings, outstanding balance, status.
  - `CustomerDetailView.tsx`: Customer profile with Multi-Event Booking History tab, Credit Limit Gauge (for Corporate Clients), Statement of Account tab, Notes.
  - `CustomerForm.tsx`: Single vs Corporate Customer form (Name, Phone, Customer Type, GSTIN, Credit Limit, Payment Term Days, Contract Discount %).
- **Leads (`crm-group/leads/`):**
  - `LeadsView.tsx`: Kanban pipeline board & list view (New → Contacted → Site Visit → Quotation → Booked → Lost).
  - `LeadDetailView.tsx` & `LeadForm.tsx`: Lead requirements capture form.
- **Site Visits (`crm-group/site-visits/`):**
  - `SiteVisitsView.tsx`, `SiteVisitDetailView.tsx`, `SiteVisitForm.tsx`: Venue measurement, power supply check, vehicle access notes, supervisor assignment.

### 3. Quotation Group Components (`components/dashboard/quotation-group/`)
- `QuotationsView.tsx`: Quotation list with status (`Draft`, `Sent`, `Approved`, `Converted`).
- `QuotationForm.tsx`: Item selection form with event date range picker, duration calculation, transport/labour charges.
- `ItemSelector.tsx`: Inventory item lookup drawer with thumbnail images, stock counts, rental rates.
- `StockAvailabilityCheck.tsx`: Real-time stock checker across 4 godowns for event date range.
- `QuotationDetailView.tsx`: Quotation PDF preview & approval trigger.

### 4. Booking Group Components (`components/dashboard/booking-group/`)
- `BookingsView.tsx`: Bookings list with event dates, location, amount, advance paid, status.
- `BookingForm.tsx`: Convert quotation to booking form, advance payment input.
- `BookingDetailView.tsx`: Comprehensive booking timeline & Godown Reservation breakdown.
- `AgreementView.tsx`: Rental contract agreement preview & PDF generator.

### 5. Warehouse Group Components (`components/dashboard/warehouse-group/`)
- **Warehouses (`warehouse-group/warehouses/`):**
  - `WarehousesView.tsx`: 4 Godowns overview cards (Main, Jaipur, Ajmer, Jodhpur) with live stock bars.
  - `WarehouseDetailView.tsx`: Godown details with Zone & Rack tabs.
  - `WarehouseForm.tsx`, `ZoneManager.tsx`, `RackManager.tsx`: Godown, Zone, and Rack setup managers.
- **Transfers (`warehouse-group/transfer/`):**
  - `StockTransfersView.tsx`, `StockTransferForm.tsx`, `TransferDetailView.tsx`: Inter-godown transfer request, approval, loading, and receiving verification.

### 6. Inventory Group Components (`components/dashboard/inventory-group/`)
- **Categories (`inventory-group/categories/`):**
  - `CategoriesView.tsx`, `CategoryForm.tsx`: Category catalog management.
- **Items (`inventory-group/items/`):**
  - `ItemsView.tsx`: Inventory items grid/table with category filters, available/reserved/damaged counts, SKU/Code badges.
  - `ItemForm.tsx`: Add/edit item form (Category, Name, Code, Unit, Purchase Cost, Rental Rate, Min Stock, Image Upload).
  - `ItemDetailView.tsx`: Item stock distribution across 4 godowns.
- **Ledger (`inventory-group/ledger/`):**
  - `LedgerView.tsx`: Complete inventory movement log (Purchase, Transfer, Reservation, Dispatch, Return, Damage, Scrap).

### 7. Operations Group Components (`components/dashboard/operations-group/`)
- **Reservation (`operations-group/reservation/`):**
  - `ReservationView.tsx`, `ReservationForm.tsx`, `WarehouseAvailability.tsx`, `ReservationSplit.tsx`: Multi-godown auto-split allocation & stock locking.
- **Dispatches (`operations-group/dispatches/`):**
  - `DispatchesView.tsx`, `DispatchForm.tsx`, `DispatchDetailView.tsx`, `LoadingChecklist.tsx`: Godown loading slips, manual checklist load verification, vehicle & driver assignment.
- **Events (`operations-group/events/`):**
  - `EventsView.tsx`, `EventDetailView.tsx`, `EventTimeline.tsx`: Active event tracking.
  - `SiteReceiptForm.tsx` & `PhotoUpload.tsx`: Site receiving confirmation & setup photo upload.
  - `PackingChecklist.tsx`, `ReturnForm.tsx`: Packing checklist & return vehicle dispatch.
  - `VerificationForm.tsx` & `DamageReport.tsx`: Godown return verification (Good, Damaged, Repair, Missing, Scrap classification).

### 8. Finance Group Components (`components/dashboard/finance-group/`)
- **Payments (`finance-group/payments/`):**
  - `PaymentsView.tsx`, `PaymentForm.tsx`, `PaymentDetailView.tsx`: Advance payment, final settlement, and bulk payment allocation across invoices.
- **Expenses (`finance-group/expenses/`):**
  - `ExpensesView.tsx`, `ExpenseForm.tsx`, `ExpenseDetailView.tsx`: Operational event expenses (Labour, Fuel, Catering).
- **Invoices (`finance-group/invoices/`):**
  - `InvoicesView.tsx`, `InvoiceDetailView.tsx`, `InvoiceBuilder.tsx`: GST Tax Invoice builder & PDF generator.
- **Cashbook & Bankbook (`finance-group/cashbook/`, `finance-group/bankbook/`):**
  - `CashbookView.tsx`, `BankbookView.tsx`: Cash and Bank ledgers with daily balance reconciliation.

### 9. Purchases Group Components (`components/dashboard/purchases-group/`)
- **Purchases (`purchases-group/purchases/`):**
  - `PurchasesView.tsx`, `PurchaseForm.tsx`, `PurchaseDetailView.tsx`: Purchase requests, PO approval, Goods Receipt (GRN).
- **Vendors (`purchases-group/vendors/`):**
  - `VendorsView.tsx`, `VendorForm.tsx`, `VendorDetailView.tsx`: Vendor directory, due alerts, payment history.

### 10. HR Group Components (`components/dashboard/hr-group/`)
- **Staff (`hr-group/staff/`):** `StaffView.tsx`, `StaffForm.tsx`, `StaffDetailView.tsx`: Employee database & wages.
- **Attendance (`hr-group/attendance/`):** `AttendanceView.tsx`: Daily & event-wise staff attendance log.
- **Vehicles (`hr-group/vehicles/`):** `VehiclesView.tsx`, `VehicleDetailView.tsx`: Vehicle fleet list, fitness/insurance dates, dispatch assignment.

### 11. Real-Time Chat Group Components (`components/dashboard/chat-group/`)
- `ChatView.tsx` & `MessagesView.tsx`: Main WhatsApp/Instagram style 2-column workspace view (`w-full h-[calc(100vh-80px)]`).
- `ChatConversationList.tsx`: Left sidebar inbox list (320px width):
  - Search Input: `Search team member...`
  - Conversation Card Item: User Avatar with status dot (`● ONLINE` emerald green / `● OFFLINE` slate grey), User Name, Timestamp ("Jul 23"), Role Badge pill (`OWNER`, `MANAGER`, `SUPERVISOR`, `STORE MANAGER`, `ACCOUNTANT`, `DRIVER`, `STAFF`), Last Message preview, Unread message badge.
  - Active State: Highlighted amber/gold border pill (`bg-amber-500/10 dark:bg-amber-500/20 border border-amber-500/30`).
- `ChatMessageWindow.tsx`: Main chat window:
  - Header Bar: User Avatar, Name, Live Status (`● ONLINE`), Action icons (Search, Attachments, User Details).
  - Chat Canvas: Soft dot grid texture surface (`bg-slate-50 dark:bg-zinc-950`).
  - Date Divider Pill: Centered date badge (`JULY 23, 2026`).
  - Received Message Bubbles: Left-aligned white/dark card pill (`bg-white dark:bg-zinc-900 border border-slate-100 dark:border-zinc-800 text-zinc-900 dark:text-zinc-100 shadow-sm`).
  - Sent Message Bubbles: Right-aligned brand accent pill (`bg-[#5C3A21] dark:bg-[#6B4627] text-white shadow-sm`) with timestamp and status double checkmarks (`11:04 AM ✓✓`).
  - Voice Note Message Bubbles (`AudioPlayerBubble.tsx`): WhatsApp-style voice note bubble with play/pause button, audio waveform visualizer, duration timer (e.g. `0:42`), and timestamp.
  - Automated Team Invite / System Bubbles: Full-width rounded notice pill (`bg-[#5C3A21]/90 text-white rounded-2xl p-3.5 shadow-md`).
  - Bottom Input Bar: Paperclip button (`Attach photo / PDF / Excel`), Microphone button (`VoiceRecorder.tsx` — live audio recording with Web Audio API), Text input (`Message team member or paste screenshot Ctrl+V`), Primary Brand Send Button (`PaperPlane` icon in `#5C3A21` / `#6B4627`).
- `ChatUserDirectory.tsx`: Multi-role user directory modal to search and start 1-on-1 chats with any portal user.
- `MediaAttachmentModal.tsx`: File upload modal for sharing site photos, PDFs, and invoices inside chat.
- `VoiceRecorder.tsx` & `AudioPlayerBubble.tsx`: Live mic recording component & WhatsApp-style audio player bubble.
- **Real-Time Engine Provider & Store Hooks:** `ChatProvider.tsx` (`provider/ChatProvider.tsx`), `useChatSocket.ts` (`hooks/useChatSocket.ts`), `chatSlice.ts` (`store/chatSlice.ts`).

### 12. Calendar, Gallery, Reports & Settings Components
- `EventCalendar.tsx`: Full interactive event calendar.
- `PhotoGrid.tsx` & `EventGalleryView.tsx`: Event setup photo gallery.
- `reports/`: `GstReportView.tsx`, `ProfitReportView.tsx`, `InventoryReportView.tsx`, `DispatchReportView.tsx`, `DamageReportView.tsx`, `EventProfitabilityView.tsx`.
- `settings/`: `CompanyProfileView.tsx`, `PreferencesView.tsx`, `UserProfileView.tsx`, `RolesListView.tsx`, `RoleFormView.tsx`, `RolesListView.tsx`, `UsersListView.tsx`, `UserFormView.tsx`.

---

## 📱 FULL RESPONSIVE DESIGN BREAKPOINTS & GRID BEHAVIOR (Mobile, Tablet, Laptop, Ultra-Wide)

The entire UI is engineered to be **100% fluid & responsive** across Mobile, Tablet, Laptop, and Full Screen viewports:

### 1. Viewport Breakpoint Matrix

| Viewport Category | Screen Width | Tailwind Class | Layout & Grid Behavior |
|---|---|---|---|
| **Mobile Portrait / Landscape** | `< 640px` | `base` / `sm` | 1-Column Stacks (`grid-cols-1`), Full-width Form Drawers (`w-full`), Slide-over Mobile Drawer Navigation toggled via TopNavBar Hamburger button, Horizontal scrollable DataTables (`overflow-x-auto`). |
| **Tablet Portrait / Landscape** | `640px - 1024px` | `md` / `lg` | 2 to 3-Column KPI Grid (`grid-cols-2 md:grid-cols-3`), Collapsible icon-only Sidebar Rail (64px width with tooltip labels), Single-column stacked Analytics Charts. |
| **Laptop / Desktop Standard** | `1024px - 1536px` | `xl` | 4 to 7-Column KPI Grid (`grid-cols-4 xl:grid-cols-7`), Expanded 260px Left Sidebar, Side-by-side Analytics Charts (`grid-cols-1 lg:grid-cols-2`). |
| **Ultra-Wide Full Screen** | `> 1536px` | `2xl` | Full 100% edge-to-edge fluid width (`w-full max-w-full px-6 py-4`), generous card grid layout, un-truncated 10-column DataTables with sticky action columns. |

### 2. Layout Container Width Rule

1. **Authentication Pages (`(auth)/login`, `(auth)/setup-wizard`):**
   - Uses **Full Screen Split Screen Layout (`w-full h-screen`)**: Left side features full-height brand hero showcase (`AuthSplitLayout.tsx`), Right side features clean full-height form drawer container.

2. **Dashboard Application Views (`(dashboard)/*`):**
   - Uses **100% Edge-to-Edge Fluid Width (`w-full max-w-full px-4 py-4 md:px-6 md:py-6`)**: No artificial max-width constraints. Data tables, KPI cards, and charts utilize full monitor width.

1. **Top KPI Cards Row:**
   - Mobile (`< 640px`): `grid-cols-1 sm:grid-cols-2 gap-3`
   - Tablet (`640px - 1024px`): `grid-cols-3 md:grid-cols-4 gap-4`
   - Laptop / Ultra-Wide (`> 1024px`): `grid-cols-4 xl:grid-cols-7 gap-4`

2. **Top Navigation Bar:**
   - Mobile: Displays Hamburger Menu button, Compact Search Icon (expands on click), Action Buttons collapse into `+` FAB or Dropdown.
   - Laptop/Desktop: Full expanded Search Input (`Ctrl + K`), visible `+ New Booking`, `Dispatch`, `Return` text buttons.

3. **DataTables & Lists:**
   - Mobile: Table rows convert to responsive stacked cards or horizontally swipeable container with fixed header column pinning.
   - Laptop: Full desktop table with inline sorting, filter chips, and pagination.

4. **Forms & Drawers:**
   - Mobile: `w-full max-w-full rounded-none` overlay modal drawer.
   - Laptop: `w-[540px] max-w-lg rounded-l-2xl` right-side slide-over drawer.
