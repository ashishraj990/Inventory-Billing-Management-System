#  Inventory-and-Billing-System
Inventory Management and Billing System in C++

## 💻 Application Output Features (System Flow)
Here is the step-by-step visual workflow of how the terminal application executes:

### 1. Primary Stock & Billing Dashboard
============================================================
               RETAIL STOCK & BILLING SYSTEM                
============================================================
1. View Active Inventory Catalog (Sorted by ID)
2. View Active Inventory Catalog (Sorted by Price)
3. Push / Add New Product Stock
4. Stage Product Item to Shopping Cart
5. Trigger Checkout Terminal (Generate Invoice)
6. Force Terminate Instance (Exit)
============================================================
Select Operation Frame (1-6): 1

### 2. View Active Inventory Catalog (Sorted by ID)
============================================================
                    CURRENT STOCK                       
------------------------------------------------------------
ID        Product Name             Stock       Price (\$)
------------------------------------------------------------
101       Logitech MX Master 3S    45          99.99
204       Sony WH-1000XM5          20          349.50
312       Keychron K2 Keyboard     15          79.00
============================================================

### 3. Stage Product Item to Shopping Cart
Enter Product ID: 101
Enter Quantity to Buy: 2
>>> Items staged to checkout vector link. <<<

### 4. Handling Error/Alert Cases (Example Cases)

* **Case A: Stock Error (Insufficient Stock Available)**
```text
Enter Product ID: 312
Enter Quantity to Buy: 20
[!] Stock Error: Insufficient stock matrix. Available: 15
```

* **Case B: Database Error (Product ID Not Found)**
```text
Enter Product ID: 999
[!] Database Error: Target ID node not found.
```

### 5. Trigger Checkout Terminal (Generate Invoice)
Select Operation Frame (1-6): 5

************************************************************
                  FINAL RETAIL INVOICE                 
************************************************************
ID        Item Name             Qty     Price     Total ($)
------------------------------------------------------------
101       Logitech MX Master 3S 2       99.99     199.98
312       Keychron K2 Keyboard  1       79.00     79.00
------------------------------------------------------------
                                    Subtotal: $278.98
                                Tax (18% GST): $50.22
============================================================
                                 GRAND TOTAL: $329.20
************************************************************

### 6. Post-Checkout Inventory Check (Stock Deducted)
============================================================
                    CURRENT STOCK                       
------------------------------------------------------------
ID        Product Name             Stock       Price ($)
------------------------------------------------------------
101       Logitech MX Master 3S    43          99.99
204       Sony WH-1000XM5          20          349.50
312       Keychron K2 Keyboard     14          79.00
============================================================

### 7. Core Thread Clean Termination
Select Operation Frame (1-6): 6

Terminating application execution thread cleanly.
