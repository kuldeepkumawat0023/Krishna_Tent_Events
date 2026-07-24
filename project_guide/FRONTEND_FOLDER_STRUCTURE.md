# Krishna Tent & Events ERP - Frontend Architecture & Directory Structure

## Overview
This document defines the modular, production-ready frontend folder structure for **Krishna Tent & Events ERP** tailored to Next.js 16 (App Router) + TypeScript + Tailwind CSS v4.

**Key Architecture Rule:** `app/` folder contains ONLY route pages (page.tsx). ALL UI component logic lives in `components/dashboard/*-group/` with sub-folders per module.

---

## Complete Frontend Folder Tree (`frontend/src/`)

```
frontend/src/
│   proxy.ts
│   
├── app
│   │   error.tsx
│   │   favicon.ico
│   │   globals.css
│   │   layout.tsx
│   │   not-found.tsx
│   │   
│   ├── (auth)
│   │   │   layout.tsx
│   │   │   
│   │   ├── login
│   │   │       page.tsx
│   │   │       
│   │   └── setup-wizard
│   │           page.tsx
│   │           
│   └── (dashboard)
│       │   layout.tsx
│       │   page.tsx
│       │   
│       ├── crm
│       │   │   page.tsx
│       │   │   
│       │   ├── customers
│       │   │   │   page.tsx
│       │   │   │   
│       │   │   ├── new
│       │   │   │       page.tsx
│       │   │   │       
│       │   │   └── [id]
│       │   │       │   page.tsx
│       │   │       │   
│       │   │       └── edit
│       │   │               page.tsx
│       │   │               
│       │   ├── leads
│       │   │   │   page.tsx
│       │   │   │   
│       │   │   ├── new
│       │   │   │       page.tsx
│       │   │   │       
│       │   │   └── [id]
│       │   │           page.tsx
│       │   │           
│       │   └── site-visits
│       │       │   page.tsx
│       │       │   
│       │       ├── new
│       │       │       page.tsx
│       │       │       
│       │       └── [id]
│       │               page.tsx
│       │               
│       ├── quotations
│       │   │   page.tsx
│       │   │   
│       │   ├── new
│       │   │       page.tsx
│       │   │       
│       │   └── [id]
│       │       │   page.tsx
│       │       │   
│       │       └── edit
│       │               page.tsx
│       │               
│       ├── bookings
│       │   │   page.tsx
│       │   │   
│       │   ├── new
│       │   │       page.tsx
│       │   │       
│       │   └── [id]
│       │       │   page.tsx
│       │       │   
│       │       └── edit
│       │               page.tsx
│       │               
│       ├── warehouses
│       │   │   page.tsx
│       │   │   
│       │   ├── new
│       │   │       page.tsx
│       │   │       
│       │   ├── transfer
│       │   │   │   page.tsx
│       │   │   │   
│       │   │   └── new
│       │   │           page.tsx
│       │   │           
│       │   └── [id]
│       │       │   page.tsx
│       │       │   
│       │       └── edit
│       │               page.tsx
│       │               
│       ├── inventory
│       │   │   page.tsx
│       │   │   
│       │   ├── categories
│       │   │   │   page.tsx
│       │   │   │   
│       │   │   └── new
│       │   │           page.tsx
│       │   │           
│       │   ├── items
│       │   │   │   page.tsx
│       │   │   │   
│       │   │   ├── new
│       │   │   │       page.tsx
│       │   │   │       
│       │   │   └── [id]
│       │   │       │   page.tsx
│       │   │       │   
│       │   │       └── edit
│       │   │               page.tsx
│       │   │               
│       │   └── ledger
│       │           page.tsx
│       │           
│       ├── reservation
│       │   │   page.tsx
│       │   │   
│       │   └── [bookingId]
│       │           page.tsx
│       │           
│       ├── dispatches
│       │   │   page.tsx
│       │   │   
│       │   ├── new
│       │   │       page.tsx
│       │   │       
│       │   └── [id]
│       │           page.tsx
│       │           
│       ├── events
│       │   │   page.tsx
│       │   │   
│       │   └── [id]
│       │       │   page.tsx
│       │       │   
│       │       ├── site-receipt
│       │       │       page.tsx
│       │       │       
│       │       ├── return
│       │       │       page.tsx
│       │       │       
│       │       └── verification
│       │               page.tsx
│       │               
│       ├── finance
│       │   │   page.tsx
│       │   │   
│       │   ├── payments
│       │   │   │   page.tsx
│       │   │   │   
│       │   │   └── new
│       │   │           page.tsx
│       │   │           
│       │   ├── expenses
│       │   │   │   page.tsx
│       │   │   │   
│       │   │   └── new
│       │   │           page.tsx
│       │   │           
│       │   ├── invoices
│       │   │   │   page.tsx
│       │   │   │   
│       │   │   └── [id]
│       │   │           page.tsx
│       │   │           
│       │   ├── cashbook
│       │   │       page.tsx
│       │   │       
│       │   └── bankbook
│       │           page.tsx
│       │           
│       ├── purchases
│       │   │   page.tsx
│       │   │   
│       │   ├── new
│       │   │       page.tsx
│       │   │       
│       │   └── [id]
│       │           page.tsx
│       │           
│       ├── vendors
│       │   │   page.tsx
│       │   │   
│       │   ├── new
│       │   │       page.tsx
│       │   │       
│       │   └── [id]
│       │       │   page.tsx
│       │       │   
│       │       └── edit
│       │               page.tsx
│       │               
│       ├── staff
│       │   │   page.tsx
│       │   │   
│       │   ├── new
│       │   │       page.tsx
│       │   │       
│       │   ├── attendance
│       │   │       page.tsx
│       │   │       
│       │   └── [id]
│       │       │   page.tsx
│       │       │   
│       │       └── edit
│       │               page.tsx
│       │               
│       ├── vehicles
│       │   │   page.tsx
│       │   │   
│       │   └── [id]
│       │           page.tsx
│       │           
│       ├── calendar
│       │       page.tsx
│       │       
│       ├── gallery
│       │   │   page.tsx
│       │   │   
│       │   └── [eventId]
│       │           page.tsx
│       │           
│       ├── notifications
│       │       page.tsx
│       │       
│       ├── chat
│       │   │   page.tsx
│       │   │   
│       │   └── [conversationId]
│       │           page.tsx
│       │           
│       ├── reports
│       │   │   page.tsx
│       │   │   
│       │   ├── gst
│       │   │       page.tsx
│       │   │       
│       │   ├── profit
│       │   │       page.tsx
│       │   │       
│       │   ├── inventory
│       │   │       page.tsx
│       │   │       
│       │   ├── dispatch
│       │   │       page.tsx
│       │   │       
│       │   ├── damage
│       │   │       page.tsx
│       │   │       
│       │   └── event-profitability
│       │           page.tsx
│       │           
│       └── settings
│           ├── company
│           │       page.tsx
│           │       
│           ├── preferences
│           │       page.tsx
│           │       
│           ├── profile
│           │       page.tsx
│           │       
│           ├── roles
│           │   │   page.tsx
│           │   │   
│           │   ├── new
│           │   │       page.tsx
│           │   │       
│           │   └── [id]
│           │       │   page.tsx
│           │       │   
│           │       └── edit
│           │               page.tsx
│           │               
│           └── users
│               │   page.tsx
│               │   
│               ├── new
│               │       page.tsx
│               │       
│               └── [id]
│                       page.tsx
│                       
├── components
│   ├── auth
│   │       ActionGuard.tsx
│   │       AuthGuard.tsx
│   │       AuthSplitLayout.tsx
│   │       LoginForm.tsx
│   │       SetupWizardForm.tsx
│   │       RoleGuard.tsx
│   │       RoutePermissionGuard.tsx
│   │       
│   ├── common
│   │       AutoScrollToTop.tsx
│   │       Button.tsx
│   │       Card.tsx
│   │       ConfirmModal.tsx
│   │       DataTable.tsx
│   │       DeleteModal.tsx
│   │       DetailViewSkeleton.tsx
│   │       FormDrawer.tsx
│   │       Input.tsx
│   │       PageHeader.tsx
│   │       Pagination.tsx
│   │       Skeleton.tsx
│   │       StatsCard.tsx
│   │       StatusBadge.tsx
│   │       ViewPageSkeleton.tsx
│   │       
│   └── dashboard
│       ├── dashboard
│       │       CalendarWidget.tsx
│       │       DashboardView.tsx
│       │       
│       ├── crm-group
│       │   ├── customers
│       │   │       CustomerDetailView.tsx
│       │   │       CustomerForm.tsx
│       │   │       CustomersView.tsx
│       │   │       
│       │   ├── leads
│       │   │       LeadDetailView.tsx
│       │   │       LeadForm.tsx
│       │   │       LeadsView.tsx
│       │   │       
│       │   └── site-visits
│       │           SiteVisitDetailView.tsx
│       │           SiteVisitForm.tsx
│       │           SiteVisitsView.tsx
│       │           
│       ├── quotation-group
│       │       QuotationDetailView.tsx
│       │       QuotationForm.tsx
│       │       QuotationsView.tsx
│       │       ItemSelector.tsx
│       │       StockAvailabilityCheck.tsx
│       │       
│       ├── booking-group
│       │       BookingDetailView.tsx
│       │       BookingForm.tsx
│       │       BookingsView.tsx
│       │       AgreementView.tsx
│       │       
│       ├── warehouse-group
│       │   ├── warehouses
│       │   │       WarehouseDetailView.tsx
│       │   │       WarehouseForm.tsx
│       │   │       WarehousesView.tsx
│       │   │       ZoneManager.tsx
│       │   │       RackManager.tsx
│       │   │       
│       │   └── transfer
│       │           StockTransferForm.tsx
│       │           StockTransfersView.tsx
│       │           TransferDetailView.tsx
│       │           
│       ├── inventory-group
│       │   ├── categories
│       │   │       CategoriesView.tsx
│       │   │       CategoryForm.tsx
│       │   │       
│       │   ├── items
│       │   │       ItemDetailView.tsx
│       │   │       ItemForm.tsx
│       │   │       ItemsView.tsx
│       │   │       
│       │   └── ledger
│       │           LedgerView.tsx
│       │           
│       ├── operations-group
│       │   ├── reservation
│       │   │       ReservationView.tsx
│       │   │       ReservationForm.tsx
│       │   │       WarehouseAvailability.tsx
│       │   │       ReservationSplit.tsx
│       │   │       
│       │   ├── dispatches
│       │   │       DispatchDetailView.tsx
│       │   │       DispatchForm.tsx
│       │   │       DispatchesView.tsx
│       │   │       LoadingChecklist.tsx
│       │   │       
│       │   └── events
│       │           EventDetailView.tsx
│       │           EventsView.tsx
│       │           EventTimeline.tsx
│       │           SiteReceiptForm.tsx
│       │           PhotoUpload.tsx
│       │           PackingChecklist.tsx
│       │           ReturnForm.tsx
│       │           VerificationForm.tsx
│       │           DamageReport.tsx
│       │           
│       ├── finance-group
│       │   ├── payments
│       │   │       PaymentDetailView.tsx
│       │   │       PaymentForm.tsx
│       │   │       PaymentsView.tsx
│       │   │       
│       │   ├── expenses
│       │   │       ExpenseDetailView.tsx
│       │   │       ExpenseForm.tsx
│       │   │       ExpensesView.tsx
│       │   │       
│       │   ├── invoices
│       │   │       InvoiceDetailView.tsx
│       │   │       InvoicesView.tsx
│       │   │       InvoiceBuilder.tsx
│       │   │       
│       │   ├── cashbook
│       │   │       CashbookView.tsx
│       │   │       
│       │   └── bankbook
│       │           BankbookView.tsx
│       │           
│       ├── purchases-group
│       │   ├── purchases
│       │   │       PurchaseDetailView.tsx
│       │   │       PurchaseForm.tsx
│       │   │       PurchasesView.tsx
│       │   │       
│       │   └── vendors
│       │           VendorDetailView.tsx
│       │           VendorForm.tsx
│       │           VendorsView.tsx
│       │           
│       ├── hr-group
│       │   ├── staff
│       │   │       StaffDetailView.tsx
│       │   │       StaffForm.tsx
│       │   │       StaffView.tsx
│       │   │       
│       │   ├── attendance
│       │   │       AttendanceView.tsx
│       │   │       
│       │   └── vehicles
│       │           VehicleDetailView.tsx
│       │           VehiclesView.tsx
│       │           
│       ├── layout
│       │       DashboardLayout.tsx
│       │       SideNavBar.tsx
│       │       TopNavBar.tsx
│       │       WarehouseSwitcher.tsx
│       │       NotificationBell.tsx
│       │       
│       ├── calendar
│       │       EventCalendar.tsx
│       │       
│       ├── gallery
│       │       PhotoGrid.tsx
│       │       EventGalleryView.tsx
│       │       
│       ├── reports
│       │       GstReportView.tsx
│       │       ProfitReportView.tsx
│       │       InventoryReportView.tsx
│       │       DispatchReportView.tsx
│       │       DamageReportView.tsx
│       │       EventProfitabilityView.tsx
│       │       
│       ├── chat-group
│       │       ChatView.tsx
│       │       ChatConversationList.tsx
│       │       ChatMessageWindow.tsx
│       │       ChatUserDirectory.tsx
│       │       MediaAttachmentModal.tsx
│       │       
│       └── settings
│               CompanyProfileView.tsx
│               PreferencesView.tsx
│               UserProfileView.tsx
│               RoleDetailView.tsx
│               RoleFormView.tsx
│               RolesListView.tsx
│               UsersListView.tsx
│               UserFormView.tsx
│               
├── contexts
│       AuthContext.tsx
│       SocketContext.tsx
│       
├── hooks
│       useAuth.ts
│       useChatSocket.ts
│       useDebounce.ts
│       usePermissions.ts
│       useSocket.ts
│       
├── lib
│   │   apiClient.ts
│   │   
│   ├── constants
│   │       permissions.ts
│   │       
│   └── services
│           ai.services.ts
│           audit.services.ts
│           auth.services.ts
│           booking.services.ts
│           chat.services.ts
│           crm.services.ts
│           dashboard.services.ts
│           dispatch.services.ts
│           event.services.ts
│           expense.services.ts
│           finance.services.ts
│           inventory.services.ts
│           invoice.services.ts
│           notification.services.ts
│           payment.services.ts
│           purchase.services.ts
│           quotation.services.ts
│           repair.services.ts
│           report.services.ts
│           reservation.services.ts
│           return.services.ts
│           role.services.ts
│           room.services.ts
│           scrap.services.ts
│           settings.services.ts
│           staff.services.ts
│           vehicle.services.ts
│           vendor.services.ts
│           verification.services.ts
│           warehouse.services.ts
│           warehouseTransfer.services.ts
│           
├── provider
│       ChatProvider.tsx
│       HydrationGuard.tsx
│       StoreProvider.tsx
│       ThemeProvider.tsx
│       
├── store
│   │   store.ts
│   │   authSlice.ts
│   │   chatSlice.ts
│   │   
│   └── hooks
│           redux.ts
│       
└── utils
        cn.ts
        formatCurrency.ts
        validations.ts
```
