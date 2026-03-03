<p align="center">
  <img src="assets/images/Icon_app.png" alt="Bayaa POS Logo" width="120" />
</p>

<h1 align="center">Bayaa POS</h1>

<p align="center">
  <strong>Enterprise Desktop Point of Sale & Retail Management System</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Flutter-3.27-02569B?style=for-the-badge&logo=flutter&logoColor=white" alt="Flutter" />
  <img src="https://img.shields.io/badge/Dart-3.6-0175C2?style=for-the-badge&logo=dart&logoColor=white" alt="Dart" />
  <img src="https://img.shields.io/badge/Platform-Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white" alt="Platform" />
  <img src="https://img.shields.io/badge/Database-SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white" alt="Database" />
  <img src="https://img.shields.io/badge/Architecture-Clean_Architecture-8B5CF6?style=for-the-badge" alt="Architecture" />
  <img src="https://img.shields.io/badge/State-BLoC/Cubit-F97316?style=for-the-badge" alt="State Management" />
</p>

---

## Overview

**Bayaa POS** is a professional Windows desktop Point of Sale application built with Flutter. Designed for **retail stores** and **mobile phone shops**, it delivers fast checkout, complete inventory control, partial refund management, and powerful analytics — all powered by an **offline-first** local SQLite database with no server required.

### Why Bayaa?

- **Zero internet dependency** — fully offline, data stays on your machine
- **Sub-second checkout** — barcode scanning (HID) + manual product search
- **Role-based access** — Manager & Cashier with distinct permission levels
- **Comprehensive reporting** — daily revenue, profit tracking, top-selling products, sales vs. refund analytics
- **Partial refund system** — item-level refunds with automatic stock restocking for both managers and cashiers
- **Print-ready** — A4 invoices and 80mm thermal receipts generated as PDF
- **Arabic RTL** — native right-to-left UI with the Amiri font

---

## Features

### Sales & POS
- Fast checkout via **HID barcode scanner** or manual product search
- Real-time cart management with quantity editing and price validation
- Minimum price enforcement to prevent below-cost sales
- A4 invoice and 80mm thermal receipt PDF generation
- One-click printing and sharing

### Invoice & Refund Management
- Full invoice history with search, date-range, and type filtering (All / Sales / Refunded)
- **Partial refund** — select specific items and quantities to return
- Automatic stock restocking on refund
- Refund invoices linked to originals with full audit trail
- Barcode scanner support for invoice search
- Refund available for both **Manager** and **Cashier** roles

### Inventory Management
- Product CRUD with barcode, pricing (retail / wholesale / cost), and stock levels
- Category-based organization
- Low-stock alerts with configurable thresholds
- Restock dialog with quick quantity update
- Paginated product list with grid and table views
- Product availability and category filters

### Reports & Analytics (ARP)
- **Daily reports** — total revenue, profit, items sold, refund summary
- **Monthly sales charts** — line and bar charts via `fl_chart`
- **Top-selling products** ranking
- **Sales vs. Refund** pie chart breakdown
- **Session history** — per-shift sales logs with close reports
- Exportable daily report summaries

### Stock Summary
- Category-level stock analysis
- Total stock value and quantity metrics
- Refunded quantity tracking per product
- Sortable data tables with search

### Session Management
- Automatic session creation on login
- Session close with summary report (total sales, items, revenue)
- Session history for managers
- Activity logging with timestamped audit trail

### Security & Roles
- **Manager** — full access: sales, invoices, refunds, products, reports, sessions, stock summary, settings, user management, data backup/restore
- **Cashier** — sales, invoices, refunds, products, stock alerts, notifications, settings (limited)
- Password-protected login with user card selection
- Permission-based UI — restricted features are hidden (not just disabled)

### Printing
- **A4 Invoice** — professional full-page layout with store branding
- **80mm Thermal Receipt** — compact receipt optimized for thermal printers
- PDF preview before printing
- Direct print and share actions

### Additional
- **License activation** gate on first launch
- **Store settings** — store name, address, phone, branding
- **User management** — add/edit/delete users with role assignment
- **Data management** — backup & restore database
- **Desktop window management** — custom title bar, minimum size enforcement
- **Low-stock notifications** — real-time badge count in sidebar
- **Activity log** — tracks sales, refunds, deletions, restocks, session events

---

## Tech Stack

| Layer | Technology | Package |
|-------|------------|---------|
| **Framework** | Flutter 3.27 (Windows Desktop) | `flutter` |
| **Language** | Dart 3.6 | — |
| **State Management** | BLoC / Cubit | `flutter_bloc` ^9.1.1, `bloc` ^9.1.0 |
| **Database** | SQLite (local, offline-first) | `sqflite_common_ffi` ^2.3.0, `sqlite3_flutter_libs` ^0.5.15 |
| **Dependency Injection** | GetIt (service locator) | `get_it` ^8.2.0 |
| **PDF Generation** | PDF creation + printing | `pdf` ^3.11.3, `printing` ^5.14.2 |
| **Barcode** | Barcode rendering + HID scanning | `barcode` ^2.2.9, `mobile_scanner` ^7.1.2 |
| **Charts** | Sales & analytics graphs | `fl_chart` ^1.1.1 |
| **Data Tables** | Enhanced paginated tables | `data_table_2` ^2.6.0 |
| **Error Handling** | Functional Either type | `dartz` ^0.10.1, `either_dart` ^1.0.0 |
| **Desktop Window** | Window size & title bar control | `window_manager` ^0.5.1 |
| **Fonts & Icons** | Arabic RTL support + icon set | `google_fonts` ^6.3.2, `lucide_icons` ^0.257.0 |
| **Utilities** | UUID, crypto, date formatting | `uuid` ^4.5.2, `crypto` ^3.0.3, `intl` ^0.20.2 |

---

## Architecture

Bayaa follows **Clean Architecture** with a feature-first folder structure:

```
lib/
├── main.dart                          # App entry point, window setup
├── secrets.dart                       # License / activation keys
├── core/                              # Shared infrastructure
│   ├── components/                    # Reusable UI widgets
│   ├── constants/                     # Colors, branding, BLoC observer
│   ├── data/
│   │   ├── models/                    # ActivityLog, StoreSettings
│   │   ├── repositories/             # Settings repository
│   │   └── services/                 # SQLite manager, persistence, backup
│   ├── di/                           # GetIt dependency injection setup
│   ├── error/                        # Failure hierarchy, error handler
│   ├── functions/                    # Toast / snackbar helpers
│   ├── logging/                      # File logger, crash logger
│   ├── security/                     # PermissionGuard (role checks)
│   ├── services/                     # ActivityLogger, printing service
│   ├── session/                      # SessionManager (auto-create shifts)
│   ├── state/                        # StateSynchronizer (event bus)
│   ├── theme/                        # AppTheme (Material 3)
│   └── utils/                        # Responsive helper, PDF report gen
└── features/
    ├── activation/                   # License activation screen
    ├── arp/                          # Analytics, Reports & Predictions
    ├── auth/                         # Login, user model, user CRUD
    ├── dashboard/                    # Sidebar navigation shell, home
    ├── invoice/                      # Invoice list, preview, refund system
    ├── notifications/                # Low-stock notification badges
    ├── products/                     # Product CRUD, grid/table views
    ├── sales/                        # POS checkout, cart, barcode scanning
    ├── sessions/                     # Session history, daily reports
    ├── settings/                     # Store info, users, data management
    ├── stock/                        # Low-stock alerts, restock dialog
    └── stock_summary/                # Category-level stock analysis
```

### Key Design Decisions

- **Cubit over Bloc** — simpler state management for CRUD-heavy screens; no event classes needed
- **Singleton cubits** via GetIt — shared state across the app (e.g., `SalesCubit`, `InvoiceCubit`)
- **Repository pattern** — abstract interfaces in `domain/`, SQLite implementations in `data/`
- **Reactive sync** — `ActivityLogger` broadcasts a stream; cubits subscribe and auto-refresh when relevant activities occur
- **`StateSynchronizer`** — lightweight event bus for cross-feature data change notifications
- **Either pattern** — all repository methods return `Either<Failure, T>` for explicit error handling

---

## Screenshots

### Login
![Login](Screenshots/login.png)

---

### Sales Interface

| POS Checkout | Cart & Search |
|:---:|:---:|
| ![Sales](Screenshots/sales.png) | ![Sales 2](Screenshots/sales2.png) |

---

### Manager Views

| Dashboard | Invoices |
|:---:|:---:|
| ![Dashboard](Screenshots/Manger/manger_dashboard.png) | ![Invoices](Screenshots/Manger/manger_invoices.png) |

| Products | Add Product |
|:---:|:---:|
| ![Products](Screenshots/Manger/manger_product.png) | ![Add Product](Screenshots/Manger/add_product.png) |

| Restock Product | Refund Invoice |
|:---:|:---:|
| ![Restock](Screenshots/Manger/reStock_product.png) | ![Refund](Screenshots/Manger/refund_invoice.png) |

| Daily Report (1) | Daily Report (2) |
|:---:|:---:|
| ![Report 1](Screenshots/Manger/daliy_report1.png) | ![Report 2](Screenshots/Manger/daliy_report2.png) |

| Daily Report (3) | Report History |
|:---:|:---:|
| ![Report 3](Screenshots/Manger/daliy_report3.png) | ![History](Screenshots/Manger/history_daliy.png) |

| Analytics (ARP) | Stock Summary (1) |
|:---:|:---:|
| ![ARP](Screenshots/Manger/Erp.png) | ![Stock 1](Screenshots/Manger/summary_stock1.png) |

| Stock Summary (2) | Stock Summary (3) |
|:---:|:---:|
| ![Stock 2](Screenshots/Manger/summary_stock2.png) | ![Stock 3](Screenshots/Manger/summary_stock3.png) |

| Settings (1) | Settings (2) | Settings (3) |
|:---:|:---:|:---:|
| ![Settings 1](Screenshots/Manger/manger_settings1.png) | ![Settings 2](Screenshots/Manger/manger_settings2.png) | ![Settings 3](Screenshots/Manger/manger_settings3.png) |

---

### Cashier Views

| Dashboard | Invoices |
|:---:|:---:|
| ![Dashboard](Screenshots/Cashier/cashier_dashboard.png) | ![Invoices](Screenshots/Cashier/cashier_invoices.png) |

| Products | Settings |
|:---:|:---:|
| ![Products](Screenshots/Cashier/cashier_products.png) | ![Settings](Screenshots/Cashier/cashier_settings.png) |

---

### Shared

| Invoice Preview | Out of Stock Alerts |
|:---:|:---:|
| ![Invoice](Screenshots/invoice.png) | ![Out of Stock](Screenshots/out_stock_products.png) |

---

## Role Permissions

| Feature | Manager | Cashier |
|---------|:-------:|:-------:|
| Sales / POS checkout | ✅ | ✅ |
| Invoice viewing | ✅ | ✅ |
| Partial refund | ✅ | ✅ |
| Invoice deletion | ✅ | ❌ |
| Bulk invoice deletion | ✅ | ❌ |
| Product management | ✅ | ✅ |
| Stock alerts | ✅ | ✅ |
| Stock summary | ✅ | ❌ |
| Reports & analytics | ✅ | ❌ |
| Session history | ✅ | ❌ |
| Notifications | ✅ | ✅ |
| User management | ✅ | ❌ |
| Store settings | ✅ | ✅ |
| Data backup / restore | ✅ | ❌ |
| Close session | ✅ | ✅ |

---

## Database

Bayaa uses a local **SQLite** database (via `sqflite_common_ffi`) that is automatically created on first launch. No server or internet connection is required.

### Tables

| Table | Purpose |
|-------|---------|
| `store_settings` | Store name, address, phone, branding |
| `categories` | Product categories |
| `products` | Product catalog (barcode, pricing, stock) |
| `users` | Manager & cashier accounts |
| `shifts` | Session open/close records |
| `sales` | Sale & refund invoices |
| `sale_items` | Line items per sale (with `refunded_quantity` tracking) |
| `activity_logs` | Timestamped audit trail of all operations |

The database schema is versioned (currently **v5**) with automatic migrations.

---

## Getting Started

### Prerequisites

- **Windows 10** or later
- **Flutter SDK 3.27+** ([install guide](https://docs.flutter.dev/get-started/install/windows/desktop))
- **Visual Studio 2022** with the **Desktop development with C++** workload

### Installation

```bash
# Clone the repository
git clone https://github.com/Desha29/Bayaa.git
cd Bayaa

# Install dependencies
flutter pub get

# Run on Windows
flutter run -d windows
```

### Build for Production

```bash
flutter build windows --release
```

The compiled executable will be in `build/windows/x64/runner/Release/`.

> **Note:** The SQLite database file is created automatically in the app's data directory on first launch. No manual database setup is needed.

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## License

This project is proprietary software. All rights reserved.

---

<p align="center">
  Built with Flutter by <a href="https://github.com/Desha29">Desha29</a>
</p>
