# Krishna Tent & Events ERP - REST API Architecture Specification

## Overview
This document defines every REST API endpoint, HTTP method, URL path, request payload, query parameter, and response payload across the **28 Express.js Route files** in `apps/backend/routes/`.

---

## 1. Authentication & User API (`/api/auth`)
- **`POST /api/auth/login`**: User authentication with email & password. Returns JWT token & user info.
- **`POST /api/auth/logout`**: Clears user session / refresh token.
- **`GET /api/auth/me`**: Get current authenticated user profile & permissions matrix.
- **`POST /api/auth/refresh-token`**: Issue new access token via refresh token.
- **`POST /api/auth/setup-wizard`**: First-time wizard configuration (Company details, default 4 godowns, admin setup).

---

## 2. Dashboard API (`/api/dashboard`)
- **`GET /api/dashboard/stats`**: Aggregated stats (Today's events, Pending payments, Low stock count, Active dispatches, Pending returns).
- **`GET /api/dashboard/calendar`**: Event calendar schedule feed.
- **`GET /api/dashboard/alerts`**: Critical ERP alerts (Low stock warnings, Payment dues, Damage approvals).

---

## 3. Warehouse Management API (`/api/warehouses`)
- **`GET /api/warehouses`**: List all godowns (Main Furniture, Jaipur, Lighting, Resort Store).
- **`POST /api/warehouses`**: Create new warehouse.
- **`GET /api/warehouses/:id`**: Warehouse detail with assigned zones, racks, & stock summary.
- **`PUT /api/warehouses/:id`**: Update warehouse details.
- **`DELETE /api/warehouses/:id`**: Soft delete warehouse.
- **`POST /api/warehouses/:id/zones`**: Add zone to warehouse.
- **`POST /api/warehouses/:id/racks`**: Add rack to zone.
- **`GET /api/warehouses/:id/stock-summary`**: Category-wise stock levels in warehouse.

---

## 4. Warehouse Stock Transfer API (`/api/warehouse-transfers`)
- **`GET /api/warehouse-transfers`**: List stock transfer records.
- **`POST /api/warehouse-transfers`**: Initiate transfer (From Warehouse → To Warehouse, Items & Quantities).
- **`PUT /api/warehouse-transfers/:id/approve`**: Owner approval for transfer.
- **`PUT /api/warehouse-transfers/:id/dispatch`**: Mark transfer items in transit.
- **`PUT /api/warehouse-transfers/:id/receive`**: Receive & update atomic stock in source and destination godowns.

---

## 5. Inventory & Item Master API (`/api/inventory`)
- **`GET /api/inventory/categories`**: List item categories (Tent, Chair, Table, Lighting, Sound, AC, Generator, Decoration).
- **`POST /api/inventory/categories`**: Create category.
- **`GET /api/inventory/items`**: List items with pagination, search, & category filter.
- **`POST /api/inventory/items`**: Create item master (Name, Code, Unit, Purchase Cost, Rental Cost, Min Stock, Barcode, Images).
- **`GET /api/inventory/items/:id`**: Item details with stock across all 4 godowns.
- **`PUT /api/inventory/items/:id`**: Update item specs.
- **`POST /api/inventory/opening-stock`**: Set opening stock per warehouse.
- **`GET /api/inventory/ledger`**: Master stock movement audit log.

---

## 6. CRM API (`/api/crm`)
- **`GET /api/crm/customers`**: List customers database.
- **`POST /api/crm/customers`**: Create customer.
- **`GET /api/crm/customers/:id`**: Customer detail with event history & ledgers.
- **`GET /api/crm/leads`**: List leads with status pipeline (New → Contacted → Site Visit → Quotation → Booking).
- **`POST /api/crm/leads`**: Create lead.
- **`PUT /api/crm/leads/:id/status`**: Update lead status.
- **`GET /api/crm/site-visits`**: List site visits.
- **`POST /api/crm/site-visits`**: Schedule site visit.

---

## 7. Quotations API (`/api/quotations`)
- **`GET /api/quotations`**: List quotations with search & date filter.
- **`POST /api/quotations`**: Create quotation with auto multi-warehouse availability check.
- **`GET /api/quotations/:id`**: Quotation detail.
- **`PUT /api/quotations/:id`**: Update quotation draft.
- **`GET /api/quotations/:id/pdf`**: Generate quotation PDF document.
- **`POST /api/quotations/:id/convert-booking`**: Convert approved quotation to confirmed Booking.

---

## 8. Bookings API (`/api/bookings`)
- **`GET /api/bookings`**: List bookings (Confirmed, Planning, In Progress, Completed, Closed).
- **`POST /api/bookings`**: Create direct booking.
- **`GET /api/bookings/:id`**: Booking full details (Items, venue, dates, payments, dispatches).
- **`PUT /api/bookings/:id`**: Update booking parameters.
- **`POST /api/bookings/:id/agreement`**: Generate & sign booking contract agreement.

---

## 9. Material Reservation API (`/api/reservations`)
- **`GET /api/reservations`**: List reservation records.
- **`GET /api/reservations/booking/:bookingId`**: Get material planning & availability per booking.
- **`POST /api/reservations`**: Lock reserved stock using auto smart-split across 4 godowns.
- **`PUT /api/reservations/:id/cancel`**: Release locked reserved stock.

---

## 10. Dispatches & Logistics API (`/api/dispatches`)
- **`GET /api/dispatches`**: List dispatches.
- **`POST /api/dispatches`**: Create dispatch record with vehicle & driver assignment.
- **`GET /api/dispatches/:id/loading-slip`**: Generate loading checklist slip per warehouse.
- **`PUT /api/dispatches/:id/scan-barcode`**: Verify item scan at loading.
- **`PUT /api/dispatches/:id/dispatch-out`**: Mark truck dispatched from warehouse.

---

## 11. Event Execution & Site Receipt API (`/api/events`)
- **`GET /api/events`**: List active on-site events.
- **`GET /api/events/:id`**: Event timeline, site photos, staff attendance.
- **`POST /api/events/:id/site-receipt`**: Confirm material receipt at event site by supervisor.
- **`POST /api/events/:id/photos`**: Upload event setup photos.
- **`POST /api/events/:id/attendance`**: Mark site staff attendance.
- **`PUT /api/events/:id/complete`**: Mark event completed & ready for packing.

---

## 12. Returns & Inspection Verification API (`/api/returns` & `/api/verifications`)
- **`POST /api/returns`**: Record return loading from event site back to warehouse.
- **`GET /api/returns/:id`**: Get return shipment details.
- **`POST /api/verifications`**: Warehouse receive & inspection verification.
  - Verification Breakdown: Available (Good), Damaged, Repair, Missing, Scrap.
- **`PUT /api/verifications/:id/approve`**: Owner approval for damage/missing write-off.

---

## 13. Finance, Payments & Invoices API (`/api/finance`)
- **`GET /api/finance/payments`**: List advance & final customer payments.
- **`POST /api/finance/payments`**: Record payment (Cash, Bank, UPI).
- **`GET /api/finance/expenses`**: List event & operational expenses.
- **`POST /api/finance/expenses`**: Record event expense.
- **`GET /api/finance/invoices`**: List GST Invoices.
- **`POST /api/finance/invoices`**: Generate invoice for booking.
- **`GET /api/finance/invoices/:id/pdf`**: Download GST Invoice PDF.
- **`GET /api/finance/cashbook`**: Daily Cash Ledger.
- **`GET /api/finance/bankbook`**: Daily Bank Ledger.
- **`GET /api/finance/profit-loss`**: P&L per event and overall business.

---

## 14. Purchases & Vendors API (`/api/purchases` & `/api/vendors`)
- **`GET /api/vendors`**: List vendors.
- **`POST /api/vendors`**: Add vendor details.
- **`GET /api/purchases/requests`**: Purchase requests.
- **`POST /api/purchases/orders`**: Create Purchase Order.
- **`POST /api/purchases/goods-receipt`**: Receive purchase goods & update inventory stock.

---

## 15. Reports API (`/api/reports`)
- **`GET /api/reports/sales`**: Sales analysis report.
- **`GET /api/reports/expenses`**: Expense analysis report.
- **`GET /api/reports/profitability`**: Event profitability report.
- **`GET /api/reports/gst`**: GST GSTR-1 & GSTR-3B summary.
- **`GET /api/reports/inventory-stock`**: Multi-godown stock status report.
- **`GET /api/reports/dispatch-register`**: Dispatch log register.
- **`GET /api/reports/damage-register`**: Damaged item history register.

---

## 16. DeepSeek AI API (`/api/ai`)
- **`POST /api/ai/theme-suggestions`**: DeepSeek AI event decoration theme recommendations based on venue & budget.
- **`POST /api/ai/cost-estimation`**: DeepSeek AI event cost & item quantity estimation.
- **`POST /api/ai/auto-checklist`**: AI generated loading & preparation checklist.

---

## 17. Settings, Roles & Audit API (`/api/settings` & `/api/audit`)
- **`GET /api/settings/company`**: Get company profile.
- **`PUT /api/settings/company`**: Update company details & logo.
- **`GET /api/settings/roles`**: List RBAC roles & permission matrix.
- **`PUT /api/settings/roles/:id`**: Update role permissions.
- **`GET /api/audit/logs`**: Audit trail logs.
