# Multi-Tenant Inventory Management System for SMEs

An offline-first, mobile Android application designed specifically for **Small and Medium Enterprises (SMEs)** to streamline inventory tracking, reduce manual stock errors, manage Point of Sale (POS) transactions, monitor stock health, and generate real-time daily reports.

Built with **Java**, **Android XML**, and a versioned **SQLite Database**, the application follows Material Design principles and provides an intuitive, context-aware navigation experience for business owners and store managers.

---

## Features & Key Functionalities

### 1. Local Authentication & Business Branding
- **User Registration & Validation**: Sign up with Business/Company Name, Email, Password, and Password Confirmation. Features email uniqueness checks and field validation.
- **Interactive Login**: Secure credential verification with touch-based password visibility toggling via compound drawables.
- **Session Persistence**: Automated session management via `SessionManager` (`SharedPreferences`).
- **Personalized Company Branding**: Displays the registered Company/Business Name dynamically across the Home/Dashboard header.

### 2. Full Inventory CRUD & Item Tracking
- **Complete Product Lifecycle**: Create, Read, Update, and Delete inventory items.
- **Comprehensive Product Attributes**: Track Name, Category, Current Quantity, Target Expected Quantity, SKU Code, Barcode, and Cost Price.
- **Auto-Generated SKUs & Barcodes**: Automatically generates unique SKUs (`SKU-XXXXXX`) and EAN Barcodes (`600XXXXXXXXX`) if left blank during product entry.
- **Modal Editing Dialogs**: Convenient `AlertDialog` interface for modifying product details without leaving the screen.

### 3. Real-Time Multi-Field Search
- Instant, dynamic filtering across **Product Name**, **SKU Code**, and **Barcode** across Inventory Management, POS Sell, and Inventory Report screens.

### 4. Point of Sale (POS) & Stock Reduction (`SellActivity`)
- **Dedicated POS Module**: Accessible via the `Sell` tab in the bottom navigation bar.
- **Available Stock Validation**: Prevents selling more items than currently in stock.
- **Out-of-Stock Handling**: Disables sales actions and visually flags items with 0 available units.
- **Atomic Database Reduction**: Reduces stock directly in SQLite upon recording a sale transaction.

### 5. Dynamic 3-Tier Stock Health Alerts
Automated percentage-based stock health categorization relative to expected target quantities:
- 🔴 **CRITICAL (< 20% Target)**: Red highlight badge — Immediate restocking required.
- 🟡 **WARNING (20% – 40% Target)**: Yellow highlight badge — Restock recommended soon.
- 🟢 **HEALTHY (≥ 40% Target)**: Green highlight badge — Optimal stock levels.

### 6. Inventory Reports & Sales Analytics
- **Inventory Metrics Overview**: Real-time aggregation of Total Products, Low-Stock Products, and Healthy Stock Products.
- **Explicit Restock Item Identification**: The Low Stock report explicitly lists the exact product names, current quantities, target baselines, and alert statuses needing restocking.
- **Daily Sales & System Summary (`SalesActivity`)**: Generates timestamped summary reports with adaptive, status-aware screen background themes:
  - 🔴 **Critical Red Background**: Triggered when critical low-stock items exist.
  - 🟡 **Warning Yellow Background**: Triggered when warning low-stock items exist.
  - 🟢 **Healthy Green Background**: Triggered when all items are healthy.
  - 🔵 **Info Blue Background**: Triggered when the inventory is empty.

### 7. Responsive UI & Nested Scrolling
- **Bottom Navigation**: Unified `BottomNavigationView` across `Home`, `Inventory`, `Sell`, and `Reports` tabs.
- **NestedScrollView Architecture**: Custom `NestedScrollView` wrappers surrounding `RecyclerView` lists ensure all inventory items are fully expanded and measured without clipping.

---

## Tech Stack & Architecture

- **Language**: Java 17
- **Min SDK**: API 24 (Android 7.0 Nougat)
- **Target SDK**: API 35 (Android 15)
- **UI Components**: Material Design 3 (`com.google.android.material`), `CardView`, `RecyclerView`, `NestedScrollView`, `BottomNavigationView`
- **Local Persistence**: SQLite (`SQLiteOpenHelper`), `SharedPreferences`
- **Build System**: Gradle (Kotlin DSL - `build.gradle.kts`)
- **Architecture Pattern**: Two-Tier Mobile Architecture (Activities, Data Models, Custom Adapters, SQLite Helper, Session Manager)

---

## Database Schema & Migrations

The local SQLite database (`Inventory.db`) utilizes a versioned schema (`DATABASE_VERSION = 5`) with automated migration handling (`onUpgrade`):

### `users` Table
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `INTEGER` | `PRIMARY KEY AUTOINCREMENT` | Unique User ID |
| `email` | `TEXT` | `UNIQUE` | User login email |
| `password` | `TEXT` | `NOT NULL` | Encrypted/Stored password |
| `business_name` | `TEXT` | | Registered Company / Business Name |

### `items` Table
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `INTEGER` | `PRIMARY KEY AUTOINCREMENT` | Unique Product ID |
| `name` | `TEXT` | `NOT NULL` | Product Name |
| `quantity` | `INTEGER` | | Current Stock Units |
| `category` | `TEXT` | `DEFAULT 'General'` | Product Category |
| `expected_quantity` | `INTEGER` | `DEFAULT 40` | Target Expected Quantity |
| `sku` | `TEXT` | | Unique Stock Keeping Unit (`SKU-XXXXXX`) |
| `barcode` | `TEXT` | | Barcode Identifier (`600XXXXXXXXX`) |
| `cost_price` | `REAL` | `DEFAULT 0.0` | Cost Price per unit |

---

## Project Structure

```
Inventory management/
├── build.gradle.kts
├── settings.gradle.kts
├── app/ (src/main/)
│   ├── AndroidManifest.xml
│   ├── java/com/example/inventoryapp/
│   │   ├── DBHelper.java             # SQLite Database Helper & Migrations (v1-v5)
│   │   ├── SessionManager.java       # SharedPreferences Session Storage
│   │   ├── Item.java                 # Domain Model & Stock Health Logic
│   │   ├── ItemAdapter.java          # RecyclerView Adapter for Inventory & Reports
│   │   ├── SellAdapter.java          # RecyclerView Adapter for POS Sales Module
│   │   ├── MainActivity.java         # Initial Launcher Entry Point
│   │   ├── LoginActivity.java        # User Authentication & Visibility Toggle
│   │   ├── RegisterActivity.java     # User Registration & Business Name Capture
│   │   ├── DashboardActivity.java    # Home Dashboard & Key Metrics
│   │   ├── InventoryActivity.java    # Full Inventory CRUD & Search
│   │   ├── SellActivity.java         # Point of Sale & Stock Reduction
│   │   ├── ReportsActivity.java      # Inventory Reports & Restock List
│   │   └── SalesActivity.java        # Daily Sales Summary & Adaptive Styling
│   └── res/
│       ├── layout/                   # Activity XML Layouts & Custom Dialogs
│       ├── menu/                     # Bottom Navigation Menu (bottom_nav_menu.xml)
│       └── values/                   # Color Palette, Strings & Themes
└── README.md
```

---

## Getting Started

### Prerequisites
- **Android Studio** Ladybug (2024.2.1) or higher
- **JDK 17** configured in Android Studio
- **Android SDK 35**
- Android Emulator or physical device running **Android 7.0 (API 24)** or higher

### Installation Steps

1. **Clone or Extract the Repository**:
   ```bash
   git clone https://github.com/your-username/inventory-management.git
   ```

2. **Open in Android Studio**:
   - Open Android Studio.
   - Select **Open** and choose the `Inventory management` project directory.

3. **Gradle Sync**:
   - Allow Android Studio to automatically download dependencies and perform a Gradle Sync.

4. **Run the Application**:
   - Select your connected Android device or emulator.
   - Click the **Run** button in Android Studio.

---

## License & Academic Attribution

Developed for **Eduvos Assessment (Module Code: ITMDA3-34 / Deliverable 4)**.  
Designed for demonstration of mobile application development, database management, and SME operational workflows.
