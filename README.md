# Java OOP Marketplace (Console)

A menu-driven **Java** project that models a simple e-commerce marketplace using classic **OOP**. It includes **buyers**, **sellers**, **items**, a **shopping cart**, and a **purchase history**. The program stores data in memory (dynamic arrays with logical/physical size) and interacts via a clean **console UI**.

> **Tech stack:** Java 8+ • Standard Library only (`java.util.Scanner`, `java.time.LocalDate`) • No external deps

---

## ✨ Features

- **Sellers & inventory**
  - Create sellers, add items, print a seller’s catalog.
  - Items contain `name` and `price` (double).

- **Buyers & shopping cart**
  - Create buyers with credentials and address (street/number/city/country).
  - Add items to a buyer’s cart, print cart contents, clear/checkout.

- **Checkout & history**
  - Calculates total price, timestamps with `LocalDate`, and stores a **snapshot** of the items in `PurchaseHistory`.

- **Manager layer**
  - Central `ManagerClass` holds arrays of buyers and sellers.
  - Automatic **resize** when logical size reaches capacity.

- **Console UI**
  - Main menu loop for all operations, input validation, and readable output.

---

## 🧱 Domain Model (Classes)

- `Item`
  - Fields: `name`, `price`
  - Methods: constructors, getters/setters, `toString()`

- `Sellers`
  - Fields: identity/credentials, `Item[] sellerItems`, logical/physical size
  - Methods: add item, print items, getters, resize helpers

- `Buyers`
  - Fields: identity + address, `Item[] shoppingCart`, `PurchaseHistory[] shoppingCartHistory`, logical/physical sizes
  - Methods: add item to cart, `checkOutShoppingCart()`, `itemsCopy()` (snapshot for history), `todayDate()`, print data

- `PurchaseHistory`
  - Fields: `Item[] items`, `double totalPrice`, `LocalDate purchaseDate`
  - Purpose: immutable-style record of a single checkout

- `ManagerClass`
  - Holds `Buyers[]` and `Sellers[]`, logical/physical sizes
  - Methods to add/list/search entities and to grow arrays

- `Shahaf_Einav_Main`
  - **Entry point**: menu loop (Scanner), routes actions to `ManagerClass` and entity methods

> The code uses **encapsulation** (private fields + getters/setters), `toString()` for readable prints, and explicit **resize** logic (allocate new array ×2 capacity and copy).

---

## 🖥️ Console Menu (example)

