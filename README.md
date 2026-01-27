# restaurant-ordering-system-design
## 📌 Features & Functions (Customer Perspective)

### 1️⃣ Feature: User Registration
Functions:
- Validate registration data
- Check email uniqueness
- Create user account
- Manage account activation
- Handle registration errors

 ---

### 2️⃣ Feature: User Login
Functions:
- Authenticate user
- Validate credentials
- Manage user session
- Handle failed login attempts
- Redirect authenticated user to home page

---

### 3️⃣ Feature: Browse Restaurants
Functions:
- Retrieve restaurant list
- Filter restaurants
- Sort restaurants
- Search restaurants
- Retrieve restaurant details

---

### 4️⃣ Feature: Browse Menu
Functions:
- Retrieve menu categories
- Retrieve menu items
- Filter menu items
- Sort menu items
- Search menu items

---

### 5️⃣ Feature: View Menu Item Details
Functions:
- Retrieve item details
- Retrieve item options
- Retrieve item availability
- Retrieve item images

---

### 6️⃣ Feature: Place Order
Functions:
- Validate order data
- Calculate order total
- Apply pricing rules (taxes, discounts)
- Create order record
- Persist order items
- Initialize order status
- Handle order submission failures

---

### 7️⃣ Feature: Customize Order
Functions:
- Manage item options
- Validate customization rules
- Calculate customized item price
- Save customization data

---

### 8️⃣ Feature: Cart Management
Functions:
- Add item to cart
- Remove item from cart
- Update cart items
- Calculate cart total
- Persist cart state
- Apply coupon or discount

---

# 📐 Analysis & Design – Cart Module

This section documents the **analysis and design artifacts** for the Cart module, focusing on the two core use cases:

- Add to Cart  
- Modify Cart

The goal is to illustrate system behavior, control flow, and interaction between components.

---

## 🛒 Add to Cart

### 📊 Flowchart
Describes the logical flow of adding an item to the cart, including cart validation, item existence check, and total recalculation.

![Add to Cart Flowchart](https://github.com/ABDULLAH1SAID/restaurant-ordering-system-design/blob/main/FoodDelivery/Followchart%20addToCart.png)

---

### 🔁 Sequence Diagram
Illustrates the interaction between the user interface, backend controller, and data entities during the add-to-cart process.

![Add to Cart Sequence](https://github.com/ABDULLAH1SAID/restaurant-ordering-system-design/blob/main/FoodDelivery/SequenceDigram%20addToCart.png)

---

### 🧠 Pseudocode
Represents the internal logic executed when a user adds an item to the cart.

---

## ✏️ Modify Cart

### 📊 Flowchart
Shows the flow for updating cart items, including increasing, decreasing, and removing items.

![Modify Cart Flowchart](https://github.com/ABDULLAH1SAID/restaurant-ordering-system-design/blob/main/FoodDelivery/Followchart%20modifyCart.png)

---

### 🔁 Sequence Diagram
Demonstrates how cart modification requests propagate through system components.

![Modify Cart Sequence](https://github.com/ABDULLAH1SAID/restaurant-ordering-system-design/blob/main/FoodDelivery/SequenceDigram%20ModifyCart.png)

---

### 🧠 Pseudocode
Defines the logical steps performed during cart modification.



