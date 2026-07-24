# Krishna Tent & Events ERP - Centralized Backend Architecture & Directory Structure

## Overview
This document defines the modular, enterprise-grade backend directory structure for **Krishna Tent & Events ERP** tailored to **Node.js + Express.js + MongoDB (Mongoose ODM)**. 

Matching the reference architecture, all core application code resides inside `backend/src/` featuring `app.js`, `middlewares/`, `services/`, `utils/`, centralized route index `routes/index.js`, clean server entry point `server.js`, and API test suite `tests/`.

---

## Complete Backend Folder Tree (`backend/`)

```
backend/
├── src
│   ├── config
│   │   ├── cloudinary.js
│   │   ├── db.js
│   │   ├── deepseek.js
│   │   ├── email.js
│   │   ├── jwt.js
│   │   ├── permissions.js
│   │   └── socket.js
│   │
│   ├── controllers
│   │   ├── aiController.js
│   │   ├── auditController.js
│   │   ├── authController.js
│   │   ├── bookingController.js
│   │   ├── chatController.js
│   │   ├── crmController.js
│   │   ├── dashboardController.js
│   │   ├── dispatchController.js
│   │   ├── eventController.js
│   │   ├── expenseController.js
│   │   ├── financeController.js
│   │   ├── inventoryController.js
│   │   ├── invoiceController.js
│   │   ├── notificationController.js
│   │   ├── paymentController.js
│   │   ├── purchaseController.js
│   │   ├── quotationController.js
│   │   ├── repairController.js
│   │   ├── reportController.js
│   │   ├── reservationController.js
│   │   ├── returnController.js
│   │   ├── roleController.js
│   │   ├── roomController.js
│   │   ├── scrapController.js
│   │   ├── settingsController.js
│   │   ├── staffController.js
│   │   ├── userController.js
│   │   ├── vehicleController.js
│   │   ├── vendorController.js
│   │   ├── verificationController.js
│   │   ├── warehouseController.js
│   │   └── warehouseTransferController.js
│   │
│   ├── middlewares
│   │   ├── auditLogger.js
│   │   ├── authMiddleware.js
│   │   ├── csrfMiddleware.js
│   │   ├── errorHandler.js
│   │   ├── rateLimiter.js
│   │   ├── rbacMiddleware.js
│   │   ├── requirePermission.js
│   │   ├── upload.js
│   │   └── validateRequest.js
│   │
│   ├── models
│   │   ├── Agreement.js
│   │   ├── Attendance.js
│   │   ├── AuditLog.js
│   │   ├── Booking.js
│   │   ├── Category.js
│   │   ├── ChatConversation.js
│   │   ├── ChatMessage.js
│   │   ├── Company.js
│   │   ├── Customer.js
│   │   ├── CustomRole.js
│   │   ├── Dispatch.js
│   │   ├── Event.js
│   │   ├── Expense.js
│   │   ├── GoodsReceipt.js
│   │   ├── InventoryLedger.js
│   │   ├── Invoice.js
│   │   ├── Item.js
│   │   ├── Lead.js
│   │   ├── LoadingSlip.js
│   │   ├── MaterialPlan.js
│   │   ├── Notification.js
│   │   ├── Payment.js
│   │   ├── PurchaseOrder.js
│   │   ├── PurchaseRequest.js
│   │   ├── Quotation.js
│   │   ├── Rack.js
│   │   ├── RepairRequest.js
│   │   ├── Reservation.js
│   │   ├── Return.js
│   │   ├── Role.js
│   │   ├── Room.js
│   │   ├── RoomBooking.js
│   │   ├── ScrapRequest.js
│   │   ├── Settings.js
│   │   ├── SiteReceipt.js
│   │   ├── SiteVisit.js
│   │   ├── Staff.js
│   │   ├── StockHistory.js
│   │   ├── User.js
│   │   ├── Vehicle.js
│   │   ├── Vendor.js
│   │   ├── Verification.js
│   │   ├── Warehouse.js
│   │   ├── WarehouseTransfer.js
│   │   └── Zone.js
│   │
│   ├── routes
│   │   ├── index.js               # Centralized Route Registry
│   │   ├── aiRoutes.js
│   │   ├── auditRoutes.js
│   │   ├── authRoutes.js
│   │   ├── bookingRoutes.js
│   │   ├── chatRoutes.js
│   │   ├── crmRoutes.js
│   │   ├── dashboardRoutes.js
│   │   ├── dispatchRoutes.js
│   │   ├── eventRoutes.js
│   │   ├── expenseRoutes.js
│   │   ├── financeRoutes.js
│   │   ├── inventoryRoutes.js
│   │   ├── invoiceRoutes.js
│   │   ├── notificationRoutes.js
│   │   ├── paymentRoutes.js
│   │   ├── purchaseRoutes.js
│   │   ├── quotationRoutes.js
│   │   ├── repairRoutes.js
│   │   ├── reportRoutes.js
│   │   ├── reservationRoutes.js
│   │   ├── returnRoutes.js
│   │   ├── roleRoutes.js
│   │   ├── roomRoutes.js
│   │   ├── scrapRoutes.js
│   │   ├── settingsRoutes.js
│   │   ├── staffRoutes.js
│   │   ├── userRoutes.js
│   │   ├── vehicleRoutes.js
│   │   ├── vendorRoutes.js
│   │   ├── verificationRoutes.js
│   │   ├── warehouseRoutes.js
│   │   └── warehouseTransferRoutes.js
│   │
│   ├── services
│   │   ├── aiService.js
│   │   ├── chatService.js
│   │   ├── dispatchService.js
│   │   ├── excelService.js
│   │   ├── invoiceService.js
│   │   ├── notificationService.js
│   │   ├── pdfService.js
│   │   ├── quotationService.js
│   │   ├── reportService.js
│   │   ├── reservationService.js
│   │   ├── returnService.js
│   │   ├── stockService.js
│   │   ├── syncService.js
│   │   └── verificationService.js
│   │
│   ├── utils
│   │   ├── apiError.js
│   │   ├── apiResponse.js
│   │   ├── asyncHandler.js
│   │   ├── constants.js
│   │   ├── emailTemplates.js
│   │   ├── formatHelpers.js
│   │   └── logger.js
│   │
│   └── app.js                     # Express Application Setup
│
├── tests
│   ├── api_unit
│   │   ├── authController.test.js
│   │   ├── bookingController.test.js
│   │   ├── crmController.test.js
│   │   ├── dashboardController.test.js
│   │   ├── dispatchController.test.js
│   │   ├── eventController.test.js
│   │   ├── expenseController.test.js
│   │   ├── financeController.test.js
│   │   ├── inventoryController.test.js
│   │   ├── invoiceController.test.js
│   │   ├── paymentController.test.js
│   │   ├── purchaseController.test.js
│   │   ├── quotationController.test.js
│   │   ├── reservationController.test.js
│   │   ├── returnController.test.js
│   │   ├── roleController.test.js
│   │   ├── settingsController.test.js
│   │   ├── staffController.test.js
│   │   ├── vendorController.test.js
│   │   ├── verificationController.test.js
│   │   └── warehouseController.test.js
│   │
│   ├── setup.js                   # Jest Test Environment Setup
│   └── testApp.js                 # Test Express App Instance
│
├── .env.example
├── generate_detailed_postman.js
├── generate_tree.js
├── jest.config.js
├── package.json
└── server.js                      # Application Listener & DB Init
```

---

## Centralized Route Registry (`src/routes/index.js`)

```js
// src/routes/index.js
const express = require('express');
const router = express.Router();

const authRoutes = require('./authRoutes');
const dashboardRoutes = require('./dashboardRoutes');
const crmRoutes = require('./crmRoutes');
const quotationRoutes = require('./quotationRoutes');
const bookingRoutes = require('./bookingRoutes');
const warehouseRoutes = require('./warehouseRoutes');
const warehouseTransferRoutes = require('./warehouseTransferRoutes');
const inventoryRoutes = require('./inventoryRoutes');
const reservationRoutes = require('./reservationRoutes');
const dispatchRoutes = require('./dispatchRoutes');
const eventRoutes = require('./eventRoutes');
const returnRoutes = require('./returnRoutes');
const verificationRoutes = require('./verificationRoutes');
const repairRoutes = require('./repairRoutes');
const scrapRoutes = require('./scrapRoutes');
const paymentRoutes = require('./paymentRoutes');
const expenseRoutes = require('./expenseRoutes');
const financeRoutes = require('./financeRoutes');
const invoiceRoutes = require('./invoiceRoutes');
const purchaseRoutes = require('./purchaseRoutes');
const vendorRoutes = require('./vendorRoutes');
const staffRoutes = require('./staffRoutes');
const vehicleRoutes = require('./vehicleRoutes');
const reportRoutes = require('./reportRoutes');
const notificationRoutes = require('./notificationRoutes');
const settingsRoutes = require('./settingsRoutes');
const auditRoutes = require('./auditRoutes');
const aiRoutes = require('./aiRoutes');
const roomRoutes = require('./roomRoutes');
const roleRoutes = require('./roleRoutes');
const userRoutes = require('./userRoutes');

router.use('/auth', authRoutes);
router.use('/dashboard', dashboardRoutes);
router.use('/crm', crmRoutes);
router.use('/quotations', quotationRoutes);
router.use('/bookings', bookingRoutes);
router.use('/warehouses', warehouseRoutes);
router.use('/warehouse-transfers', warehouseTransferRoutes);
router.use('/inventory', inventoryRoutes);
router.use('/reservations', reservationRoutes);
router.use('/dispatches', dispatchRoutes);
router.use('/events', eventRoutes);
router.use('/returns', returnRoutes);
router.use('/verifications', verificationRoutes);
router.use('/repairs', repairRoutes);
router.use('/scraps', scrapRoutes);
router.use('/payments', paymentRoutes);
router.use('/expenses', expenseRoutes);
router.use('/finance', financeRoutes);
router.use('/invoices', invoiceRoutes);
router.use('/purchases', purchaseRoutes);
router.use('/vendors', vendorRoutes);
router.use('/staff', staffRoutes);
router.use('/vehicles', vehicleRoutes);
router.use('/reports', reportRoutes);
router.use('/notifications', notificationRoutes);
router.use('/settings', settingsRoutes);
router.use('/audit', auditRoutes);
router.use('/ai', aiRoutes);
router.use('/rooms', roomRoutes);
router.use('/roles', roleRoutes);
router.use('/users', userRoutes);

module.exports = router;
```
