#include <iostream>
#include <string>
#include <iomanip>
#include <limits>

using namespace std;

// ============================================================================
// 1. OOP LAYER: PRODUCT ENTITY (Encapsulation)
// ============================================================================
class Product {
private:
    int id;
    string name;
    int quantity;
    double price;

public:
    Product(int p_id = 0, string p_name = "", int p_qty = 0, double p_price = 0.0) 
        : id(p_id), name(p_name), quantity(p_qty), price(p_price) {}

    // Getters & Setters
    int getId() const { return id; }
    string getName() const { return name; }
    int getQuantity() const { return quantity; }
    double getPrice() const { return price; }
    
    void setQuantity(int qty) { quantity = qty; }
    void setPrice(double prc) { price = prc; }
    void setName(string nm) { name = nm; }

    bool deductStock(int qty) {
        if (qty <= quantity) {
            quantity -= qty;
            return true;
        }
        return false;
    }
};

// ============================================================================
// 2. DSA LAYER: HASH TABLE WITH CHAINING (O(1) Search, Insert, Delete)
// ============================================================================
class HashNode {
public:
    Product data;
    HashNode* next;
    HashNode(Product prod) : data(prod), next(nullptr) {}
};

class InventoryHash {
private:
    static const int TABLE_SIZE = 19; 
    HashNode* table[TABLE_SIZE];

    int hashFunction(int key) const {
        return key % TABLE_SIZE;
    }

public:
    InventoryHash() {
        for (int i = 0; i < TABLE_SIZE; i++) table[i] = nullptr;
    }

    void insert(Product prod) {
        int index = hashFunction(prod.getId());
        HashNode* newNode = new HashNode(prod);
        newNode->next = table[index]; 
        table[index] = newNode;
    }

    Product* search(int id) {
        int index = hashFunction(id);
        HashNode* current = table[index];
        while (current != nullptr) {
            if (current->data.getId() == id) return &(current->data);
            current = current->next;
        }
        return nullptr;
    }

    bool remove(int id) {
        int index = hashFunction(id);
        HashNode* current = table[index];
        HashNode* prev = nullptr;

        while (current != nullptr) {
            if (current->data.getId() == id) {
                if (prev == nullptr) table[index] = current->next;
                else prev->next = current->next;
                delete current;
                return true;
            }
            prev = current;
            current = current->next;
        }
        return false;
    }

    // Dynamic array collection helper for sorting operations
    int collectProducts(Product arr[]) {
        int count = 0;
        for (int i = 0; i < TABLE_SIZE; i++) {
            HashNode* current = table[i];
            while (current != nullptr) {
                arr[count++] = current->data;
                current = current->next;
            }
        }
        return count;
    }

    ~InventoryHash() {
        for (int i = 0; i < TABLE_SIZE; i++) {
            HashNode* current = table[i];
            while (current != nullptr) {
                HashNode* temp = current;
                current = current->next;
                delete temp;
            }
        }
    }
};

// ============================================================================
// 3. DSA LAYER: MERGE SORT IMPLEMENTATION (O(N log N) Sorting)
// ============================================================================
class InventorySorter {
private:
    static void merge(Product arr[], int left, int mid, int right, bool sortByPrice) {
        int n1 = mid - left + 1;
        int n2 = right - mid;

        Product* L = new Product[n1];
        Product* R = new Product[n2];

        for (int i = 0; i < n1; i++) L[i] = arr[left + i];
        for (int j = 0; j < n2; j++) R[j] = arr[mid + 1 + j];

        int i = 0, j = 0, k = left;
        while (i < n1 && j < n2) {
            bool condition = sortByPrice ? (L[i].getPrice() <= R[j].getPrice()) 
                                         : (L[i].getId() <= R[j].getId());
            if (condition) arr[k++] = L[i++];
            else arr[k++] = R[j++];
        }

        while (i < n1) arr[k++] = L[i++];
        while (j < n2) arr[k++] = R[j++];

        delete[] L;
        delete[] R;
    }

public:
    static void mergeSort(Product arr[], int left, int right, bool sortByPrice) {
        if (left >= right) return;
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid, sortByPrice);
        mergeSort(arr, mid + 1, right, sortByPrice);
        merge(arr, left, mid, right, sortByPrice);
    }
};

// ============================================================================
// 4. DSA LAYER: LINKED LIST FOR CART MANAGEMENT
// ============================================================================
struct CartNode {
    Product item;
    int qtyBought;
    CartNode* next;
    CartNode(Product p, int q) : item(p), qtyBought(q), next(nullptr) {}
};

class ShoppingCart {
private:
    CartNode* head;

public:
    ShoppingCart() : head(nullptr) {}

    void addToCart(Product p, int qty) {
        CartNode* newNode = new CartNode(p, qty);
        if (!head) {
            head = newNode;
        } else {
            CartNode* temp = head;
            while (temp->next) temp = temp->next;
            temp->next = newNode;
        }
    }

    bool isEmpty() const { return head == nullptr; }

    void checkout(double taxRate = 0.18) {
        if (!head) return;

        double subtotal = 0.0;
        cout << "\n\n" << string(60, '*') << "\n";
        cout << "                  FINAL RETAIL INVOICE                 \n";
        cout << string(60, '*') << "\n";
        cout << left << setw(10) << "ID" << setw(22) << "Item Name" << setw(8) << "Qty" << setw(10) << "Price" << "Total ($)\n";
        cout << string(60, '-') << "\n";

        CartNode* temp = head;
        while (temp != nullptr) {
            double cost = temp->item.getPrice() * temp->qtyBought;
            subtotal += cost;

            cout << left << setw(10) << temp->item.getId()
                 << setw(22) << temp->item.getName()
                 << setw(8) << temp->qtyBought
                 << fixed << setprecision(2) << setw(10) << temp->item.getPrice()
                 << cost << "\n";

            CartNode* oldNode = temp;
            temp = temp->next;
            delete oldNode; 
        }
        head = nullptr; 

        double tax = subtotal * taxRate;
        double total = subtotal + tax;

        cout << string(60, '-') << "\n";
        cout << right << setw(45) << "Subtotal: $" << subtotal << "\n";
        cout << right << setw(45) << "Tax (18% GST): $" << tax << "\n";
        cout << string(60, '=') << "\n";
        cout << right << setw(45) << "GRAND TOTAL: $" << total << "\n";
        cout << string(60, '*') << "\n\n";
    }
};

// ============================================================================
// 5. APPLICATION CONTROLLER INTERFACE
// ============================================================================
void displayInventory(InventoryHash& inv, bool sortByPrice = false) {
    Product tempArr[100]; 
    int count = inv.collectProducts(tempArr);

    if (count == 0) {
        cout << "\n[Notice]: The stock inventory catalog is empty.\n";
        return;
    }

    InventorySorter::mergeSort(tempArr, 0, count - 1, sortByPrice);

    cout << "\n" << string(60, '=') << "\n";
    cout << "                    CURRENT STOCK                       \n";
    cout << string(60, '-') << "\n";
    cout << left << setw(10) << "ID" << setw(25) << "Product Name" << setw(12) << "Stock" << "Price ($)\n";
    cout << string(60, '-') << "\n";
    for (int i = 0; i < count; i++) {
        cout << left << setw(10) << tempArr[i].getId()
             << setw(25) << tempArr[i].getName()
             << setw(12) << tempArr[i].getQuantity()
             << fixed << setprecision(2) << tempArr[i].getPrice() << "\n";
    }
    cout << string(60, '=') << "\n";
}

int main() {
    InventoryHash inventory;
    int choice;

    // Seed Data
    inventory.insert(Product(103, "Gaming Laptop", 10, 999.99));
    inventory.insert(Product(101, "Wireless Mouse", 50, 25.50));
    inventory.insert(Product(104, "Mechanical Keyboard", 35, 89.00));
    inventory.insert(Product(102, "FHD Monitor 27\"", 15, 175.00));

    while (true) {
        cout << "=======================================\n";
        cout << "   INVENTORY & BILLING MANAGEMENT SYSTEM \n";
        cout << "=======================================\n";
        cout << "1. View Catalog (Sorted by ID)\n";
        cout << "2. View Catalog (Sorted by Price)\n";
        cout << "3. Insert New Inventory Product Entry\n";
        cout << "4. Delete Product Row item from Database\n";
        cout << "5. Initialize Customer Counter Bill\n";
        cout << "6. Terminate Application runtime\n";
        cout << "---------------------------------------\n";
        cout << "Select Options Index (1-6): ";

        if (!(cin >> choice)) {
            cout << "\n[Error]: Invalid choice stream type entry.\n\n";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            continue;
        }

        if (choice == 6) break;

        switch (choice) {
            case 1:
                displayInventory(inventory, false);
                break;
            case 2:
                displayInventory(inventory, true);
                break;
            case 3: {
                int id, qty; string name; double price;
                cout << "Enter Product ID: "; cin >> id;
                if (inventory.search(id)) {
                    cout << "[Error]: Unique primary key collision. Product ID exists.\n\n";
break;
}
cin.ignore();
cout << "Enter Product Name: "; getline(cin, name);
cout << "Enter Stock Quantity: "; cin >> qty;
cout << "Enter Unit Price: "; cin >> price;
inventory.insert(Product(id, name, qty, price));
cout << "[Success]: Hashed item node successfully pushed to database.\n\n";
break;
}
case 4: {
int id;
cout << "Enter Target Product ID to erase: "; cin >> id;
if (inventory.remove(id)) cout << "[Success]: Item deleted cleanly.\n\n";
else cout << "[Error]: Element not identified.\n\n";
break;
}
case 5: {
ShoppingCart cart;
int id, qty; char nextItem;
do {
displayInventory(inventory, false);
cout << "Enter Target Product ID to buy: "; cin >> id;
Product* target = inventory.search(id);
if (!target) {
cout << "[Error]: Search execution returned a missing index status.\n";
} else {
cout << "Selected: " << target->getName() << " | In-Stock: " << target->getQuantity() << "\n";
cout << "Quantity: "; cin >> qty;
if (qty > 0 && target->deductStock(qty)) {
cart.addToCart(*target, qty);
cout << "[Cart]: Linked list chain updated.\n";
} else {
cout << "[Error]: Out of stock bounds selection rejected.\n";
}
}
cout << "Add another product entry row? (y/n): "; cin >> nextItem;
} while (nextItem == 'y' || nextItem == 'Y');
if (!cart.isEmpty()) cart.checkout();
break;
}
default:
cout << "[Error]: Selection fallback triggered. Input range out of bounds.\n\n";
}
}
return 0;
}
