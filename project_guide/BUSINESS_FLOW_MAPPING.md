# Krishna Tent & Events ERP - Business Flow & Architecture Mapping

## Executive Summary
This document provides a comprehensive mapping between the **Business Operations Flow** (7 Departments, Core ERP Backbone, Daily Role Workflows, and Financial Flow) and the **Technical System Architecture** (Frontend Routes, Component Groups, Backend API Services, and Database Ledger Models) for **Krishna Tent & Events ERP**.

---

## 1. 7 Business Department Flows vs Technical Architecture

### Department 1: Warehouse Management Flow
*Flow:* Dashboard → Warehouse → Check Stock → Low Stock Alert → Purchase Request → Owner Approval → PO → Vendor → Goods Receive → Quality Check → Warehouse Entry → Inventory Update → Available Stock

| Technical Layer | Implementation |
|---|---|
| **Frontend Routes** | `/warehouses`, `/warehouses/[id]`, `/warehouses/new`, `/warehouses/transfer` |
| **Component Sub-folders** | `components/dashboard/warehouse-group/warehouses/` (`WarehousesView`, `WarehouseDetailView`, `WarehouseForm`, `ZoneManager`, `RackManager`) |
| **Backend API Routes** | `/api/warehouses` (`warehouse.routes.js`) |
| **Services & Ledger** | `warehouse.services.ts`, `Warehouse` Model, `StockLedger` Model |

---

### Department 2: New Item Add Flow
*Flow:* Owner → Inventory → Add Item (Category, Name, Code, Unit, Purchase Cost, Rental Cost, Min Stock, Barcode) → Save → Warehouse Select → Opening Stock → Ready (Item appears in Quotations)

| Technical Layer | Implementation |
|---|---|
| **Frontend Routes** | `/inventory`, `/inventory/items/new`, `/inventory/categories/new`, `/inventory/ledger` |
| **Component Sub-folders** | `components/dashboard/inventory-group/items/` (`ItemsView`, `ItemForm`, `ItemDetailView`), `components/dashboard/inventory-group/categories/` |
| **Backend API Routes** | `/api/inventory` (`inventory.routes.js`) |
| **Services & Ledger** | `inventory.services.ts`, `Item` Model, `Category` Model, `InventoryLedger` Model |

---

### Department 3: Warehouse Transfer Flow
*Flow:* Owner → Transfer Stock (From Warehouse → To Warehouse, Item, Qty) → Approval → Loading → Dispatch → Receive → Stock Updated (Both Warehouses)

| Technical Layer | Implementation |
|---|---|
| **Frontend Routes** | `/warehouses/transfer`, `/warehouses/transfer/new` |
| **Component Sub-folders** | `components/dashboard/warehouse-group/transfer/` (`StockTransfersView`, `StockTransferForm`, `TransferDetailView`) |
| **Backend API Routes** | `/api/warehouse-transfers` (`warehouseTransfer.routes.js`) |
| **Services & Ledger** | `warehouseTransfer.services.ts`, `WarehouseTransfer` Model, Dual-Update Stock Ledger |

---

### Department 4: Repair Management Flow
*Flow:* Warehouse Broken Item → Store Manager → Repair Request → Owner Approve → Repair Vendor → Repair Complete → Warehouse Entry → Available Again

| Technical Layer | Implementation |
|---|---|
| **Frontend Routes** | Handled within operations/warehouse dashboard |
| **Component Sub-folders** | `components/dashboard/operations-group/events/` (`DamageReport`, `VerificationForm`) |
| **Backend API Routes** | `/api/repairs` (`repair.routes.js`) |
| **Services & Ledger** | `repair.services.ts`, `Repair` Model, Stock status change (Damaged → Repair → Available) |

---

### Department 5: Scrap Management Flow
*Flow:* Repair Not Possible → Scrap Request → Owner Approval → Inventory Remove → Scrap Sale Entry → Finance Entry

| Technical Layer | Implementation |
|---|---|
| **Frontend Routes** | Integrated in Inventory & Finance views |
| **Component Sub-folders** | `components/dashboard/inventory-group/items/`, `components/dashboard/finance-group/expenses/` |
| **Backend API Routes** | `/api/scraps` (`scrap.routes.js`) |
| **Services & Ledger** | `scrap.services.ts`, `Scrap` Model, Financial Cashbook Credit |

---

### Department 6: Purchase Management Flow
*Flow:* Vendor → Purchase Order → Goods Receive → Invoice Upload → Payment → Inventory Update → Reports

| Technical Layer | Implementation |
|---|---|
| **Frontend Routes** | `/purchases`, `/purchases/new`, `/purchases/[id]`, `/vendors`, `/vendors/new` |
| **Component Sub-folders** | `components/dashboard/purchases-group/purchases/`, `components/dashboard/purchases-group/vendors/` |
| **Backend API Routes** | `/api/purchases` (`purchase.routes.js`), `/api/vendors` (`vendor.routes.js`) |
| **Services & Ledger** | `purchase.services.ts`, `vendor.services.ts`, `PurchaseOrder` Model, `Vendor` Model |

---

### Department 7: Event Order Flow (Main Revenue Engine)
*Flow:* Customer Call → CRM → Site Visit → Requirement Discussion → Event Planning → Stock Availability Check → Quotation → Customer Approval → Booking → Advance Payment → Reservation (Multi-warehouse) → Loading Slip → Barcode Scan → Vehicle Loading → Dispatch → Site Receive → Setup → Event Complete → Packing → Return → Verification → Available

| Step | Technical Route | Component | Backend Service |
|---|---|---|---|
| **1. Lead & Site Visit** | `/crm/leads`, `/crm/site-visits` | `crm-group/leads/`, `crm-group/site-visits/` | `crm.services.ts` |
| **2. Stock Check & Quotation** | `/quotations/new` | `quotation-group/` (`StockAvailabilityCheck`, `QuotationForm`) | `quotation.services.ts` |
| **3. Booking & Advance** | `/bookings/new` | `booking-group/` (`BookingForm`, `AgreementView`) | `booking.services.ts` |
| **4. Reservation & Split** | `/reservation/[bookingId]` | `operations-group/reservation/` (`ReservationSplit`) | `reservation.services.ts` |
| **5. Loading & Dispatch** | `/dispatches/new` | `operations-group/dispatches/` (`LoadingChecklist`) | `dispatch.services.ts` |
| **6. Site Setup & Photos** | `/events/[id]/site-receipt` | `operations-group/events/` (`SiteReceiptForm`, `PhotoUpload`) | `event.services.ts` |
| **7. Return & Packing** | `/events/[id]/return` | `operations-group/events/` (`PackingChecklist`, `ReturnForm`) | `return.services.ts` |
| **8. Verification & Damage** | `/events/[id]/verification` | `operations-group/events/` (`VerificationForm`, `DamageReport`) | `verification.services.ts` |

---

## 2. Core ERP Backbone Lifecycle Flow

```
┌─────────────┐     ┌─────────────────┐     ┌───────────────┐     ┌─────────────┐
│ WAREHOUSE   │ ──► │ AVAILABLE STOCK │ ──► │ EVENT BOOKING │ ──► │ RESERVATION │
└─────────────┘     └─────────────────┘     └───────────────┘     └─────────────┘
                                                                         │
┌─────────────┐     ┌─────────────────┐     ┌───────────────┐            ▼
│ AVAILABLE   │ ◄── │  VERIFICATION   │ ◄── │    RETURN     │ ◄── ┌─────────────┐
│    AGAIN    │     └─────────────────┘     └───────────────┘     │  DISPATCH & │
└─────────────┘                                                   │ SITE SETUP  │
                                                                  └─────────────┘
```

---

## 3. Role-Based Daily Workflows & UI Access

| Role | Morning Workflow | Key Components Accessed | API Authorization Scope |
|---|---|---|---|
| **Owner** | Check events, pending dispatches, stock levels, approve purchases/transfers, review evening P&L | `DashboardView`, `CompanyProfileView`, `ReportsView` | Full System Admin Rights |
| **Store Manager** | Receive purchases, update stock, reserve material, generate loading slips, verify returns | `WarehousesView`, `ItemsView`, `ReservationView`, `DispatchesView` | Warehouse & Inventory Read/Write |
| **Supervisor** | Site arrival, receive material, setup execution, photo uploads, staff site attendance, packing & return | `EventsView`, `SiteReceiptForm`, `PhotoUpload`, `AttendanceView` | Event & Execution Scope |
| **Accountant** | Record booking advance, vendor payments, expense logs, cashbook/bankbook updates, GST invoices | `PaymentsView`, `ExpensesView`, `InvoicesView`, `CashbookView` | Financial Ledgers & Billing |

---

## 4. Complete Finance & Billing Flow

```
Customer Booking ──► Advance Payment Entry ──► Event Expenses Logged ──► Vendor Payments Settled 
                         │
                         ▼
             Final Customer Payment ──► GST Invoice Generation ──► Cashbook/Bankbook ──► Event P&L Report
```

| Financial Module | Frontend Path | Component | Service |
|---|---|---|---|
| Payments | `/finance/payments` | `finance-group/payments/` | `payment.services.ts` |
| Expenses | `/finance/expenses` | `finance-group/expenses/` | `expense.services.ts` |
| Invoices | `/finance/invoices/[id]` | `finance-group/invoices/` | `invoice.services.ts` |
| Cash Book | `/finance/cashbook` | `finance-group/cashbook/` | `finance.services.ts` |
| Bank Book | `/finance/bankbook` | `finance-group/bankbook/` | `finance.services.ts` |
