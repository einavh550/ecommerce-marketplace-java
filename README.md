# Java OOP Marketplace (Console)

A menu-driven Java project that models a simple e-commerce marketplace using classic **OOP** and clean console I/O. It implements **buyers**, **sellers**, **items**, a **shopping cart**, and **purchase history**, with data stored in memory via dynamic arrays (logical vs. physical size). The design emphasizes encapsulation, separation of concerns, and readable output.

**Tech stack:** Java 8+ · Standard Library only (`java.util.Scanner`, `java.time.LocalDate`) · No external dependencies

---

## Highlights (What it does)
- **Sellers & inventory** — create sellers, add items, list a seller’s catalog.
- **Buyers & cart** — create buyers (credentials + full address), add items to cart, view cart, checkout.
- **Checkout & history** — compute totals, timestamp with `LocalDate`, store an immutable snapshot in `PurchaseHistory`.
- **Manager layer** — central orchestration that owns the buyers/sellers collections and **auto-resizes** them.
- **Console UI** — robust menu loop (Scanner), validation, and human-friendly printing.

---

## Domain Model (Classes & Responsibilities)

- `Item`  
  Fields: `name`, `price`.  
  Ops: getters/setters, `toString()` for pretty printing.

- `Sellers`  
  Fields: `sellerName`, `sellerPassword`, `Item[] sellerItems`, logical/physical sizes for the catalog.  
  Ops: `addItem(...)`, `printItems()`, `toString()`, internal **resize** when the catalog is full.

- `Buyers`  
  Fields: identity + address (`street`, `number`, `city`, `country`), `Item[] shoppingCart`, `PurchaseHistory[] shoppingCartHistory`, logical/physical sizes for both.  
  Ops: add item to cart, `checkOutShoppingCart()`, `itemsCopy()` (deep snapshot of cart for the receipt), `todayDate()` (returns `LocalDate.now()`), `printShoppingCartHistory()`, `toString()`.

- `PurchaseHistory`  
  Fields: immutable-style **snapshot** of one checkout — `Item[] items`, `double totalPrice`, `LocalDate purchaseDate`.  
  Purpose: preserve receipts even if inventory/cart later changes.

- `ManagerClass`  
  Fields: `Buyers[] buyers`, `Sellers[] sellers`, each with logical/physical sizes; index helpers.  
  Ops: add/list/search entities, and guards like `checkIfLogicalEqualsPhysicalBuyer/Seller()` to **double capacity** and copy when needed.

- `Shahaf_Einav_Main` (entry point)  
  Ops: menu loop, input parsing/validation, delegation to `ManagerClass` and entities; keeps UI logic out of the domain.

---

## Console Flow (Example)

Welcome to the Java OOP Marketplace  
0) Exit  
1) Add seller  
2) Add buyer  
3) Add item to seller  
4) Add item to buyer’s cart  
5) Checkout buyer’s cart  
6) Show all buyers  
7) Show seller data  
Choose: _

Input is validated (index in range, non-empty names, non-negative prices). On checkout the program prints date, items, and total (NIS), writes a `PurchaseHistory` record, and clears the cart.

---

## Design & Implementation Notes

- **Encapsulation:** fields are private with getters/setters; printing is centralized via `toString()` to keep UI tidy.  
- **Dynamic arrays:** every collection tracks **logical** (used) vs **physical** (capacity). When full, a new array with ×2 capacity is allocated and elements are copied (amortized O(1) append). This pattern appears in: sellers list, buyers list, each seller’s items, each buyer’s cart, and the buyer’s history.  
- **Immutability of receipts:** checkout uses `itemsCopy()` to freeze the cart contents in `PurchaseHistory` so past receipts never change.  
- **Separation of concerns:** `ManagerClass` orchestrates collections; entities own their behavior; the main class handles I/O only.  
- **Readability & safety:** clear prompts, defensive checks before indexing arrays, and consistent formatting.  
- **Time handling:** `LocalDate` is used for simple, timezone-free purchase dates.

---

## Build & Run

**Package name:** `Shahaf_Einav`

Recommended layout:
- `src/Shahaf_Einav/Buyers.java`  
- `src/Shahaf_Einav/Sellers.java`  
- `src/Shahaf_Einav/Item.java`  
- `src/Shahaf_Einav/PurchaseHistory.java`  
- `src/Shahaf_Einav/ManagerClass.java`  
- `src/Shahaf_Einav/Shahaf_Einav_Main.java`

CLI:
- Compile: `javac -d out src/Shahaf_Einav/*.java`  
- Run: `java -cp out Shahaf_Einav.Shahaf_Einav_Main`

IDE:
- Create a Java project, set the package to `Shahaf_Einav`.  
- Set the Main class to `Shahaf_Einav.Shahaf_Einav_Main`.  
- Run.

---

## Why this project is meaningful (What I learned)

- **Core OOP:** classes, encapsulation, composition, and clean `toString()` output.  
- **Manual data structures:** implementing **logical vs physical** sizing and **resize** strategies without collections — understanding memory, copying, and amortized cost.  
- **Robust console UX:** menu design, Scanner input validation, and predictable control flow.  
- **Domain modeling:** buyers/sellers/items mapped to clear classes with single responsibilities.  
- **Auditability:** immutable purchase history via snapshots and dates.  
- **Extensibility:** a layered design (UI → manager → entities) that makes it straightforward to add features.

---

## Roadmap / Possible Improvements

- Replace arrays with `ArrayList`/`List<Map>` and generics; add search/sort by name/price.  
- Persistence layer (CSV/JSON files) and loading on startup.  
- Simple authentication (login) and stronger validation utilities.  
- Currency/locale formatting via `NumberFormat`.  
- Unit tests for manager operations and checkout logic.

---

