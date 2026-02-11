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
### 📌 Food Delivery System – Database Design (ERD)

This diagram represents the **complete database design** for a Food Delivery System, including:

- **User Management:** Handles users, roles, customers, addresses, and preferred payment settings.
- **Restaurant & Menu:** Captures restaurants, their details, menus, and menu items.
- **Cart & Order Management:** Supports customer carts, orders, order items, and order statuses.
- **Payment System:** Integrates multiple payment methods, transaction records, configurations, and statuses.
- **Auditing:** Logs user actions and changes across the system for tracking and accountability.

This ERD provides a full overview of the entities, relationships, and structure needed for a scalable and production-ready food delivery platform.


![Food Delivery DB Design](https://github.com/ABDULLAH1SAID/restaurant-ordering-system-design/blob/main/FoodDelivery/FOOD_DELIVERYERD.png)

--- 

# 📐 Analysis & Design – Cart Module

This section presents the **analysis and design of the Cart module**, covering both the **data layer** and **behavioral aspects** of the system.

It includes:
- Control flow and system interactions
- Core business logic for cart operations

The analysis focuses on the following core use cases:
- Add to Cart  
- Modify Cart  

The goal is to provide a clear understanding of how the cart is structured, how data flows through the system, and how different components interact to support real-world e-commerce scenarios.

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

```pseudo
// Main Function: Add Item to Cart
FUNCTION addToCart(userId, itemId, quantity = 1)
    // Step 1: Check if cart exists for user
    cart = findCartByUserId(userId)
    
    // Step 2: If no cart exists, create one
    IF cart IS NULL THEN
        cart = createCart(userId)
    END IF
    
    // Step 3: Check if item already exists in cart
    cartItem = findCartItem(cart.id, itemId)
    
    // Step 4: Add or Update item quantity
    IF cartItem IS NOT NULL THEN
        // Item exists - increase quantity
        cartItem.quantity = cartItem.quantity + quantity
        updateCartItem(cartItem)
    ELSE
        // New item - add to cart
        addNewCartItem(cart.id, itemId, quantity)
    END IF
    
    // Step 5: Update cart total
    newTotal = calculateCartTotal(cart.id)
    updateCartTotal(cart.id, newTotal)
    
    // Step 6: Return success
    RETURN {
        success: true,
        message: "Item added to cart",
        cartTotal: newTotal
    }
END FUNCTION
```

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

```pseudo
// Main Function: Modify Cart Item
FUNCTION modifyCartItem(userId, itemId, action)
    // Step 1: Get user's cart
    cart = getCartByUserId(userId)
    
    IF cart IS NULL THEN
        RETURN {
            success: false,
            error: "NO_CART",
            message: "Cart not found"
        }
    END IF
    
    // Step 2: Get item from cart
    cartItem = getCartItem(cart.id, itemId)
    
    IF cartItem IS NULL THEN
        RETURN {
            success: false,
            error: "ITEM_NOT_IN_CART",
            message: "Item not in your cart"
        }
    END IF
    
    // Step 3: Execute selected action
    IF action == "INCREASE" THEN
        result = increaseItemQuantity(cartItem)
        
    ELSE IF action == "DECREASE" THEN
        result = decreaseItemQuantity(cartItem)
        
    ELSE IF action == "REMOVE" THEN
        result = removeItemFromCart(cartItem)
        
    ELSE
        RETURN {
            success: false,
            error: "INVALID_ACTION",
            message: "Invalid action requested"
        }
    END IF
    
    // Step 4: If action failed, return error
    IF result.success == false THEN
        RETURN result
    END IF
    
    // Step 5: Recalculate cart total
    newTotal = recalculateCartTotal(cart.id)
    
    // Step 6: Return success
    RETURN {
        success: true,
        action: action,
        newTotal: newTotal,
        itemId: itemId,
        message: getSuccessMessage(action, cartItem)
    }
END FUNCTION
```
---

### 📌 Food Delivery System – Database Design (ERD)

This diagram represents the **complete database design** for a Food Delivery System, including:

- **User Management:** Handles users, roles, customers, addresses, and preferred payment settings.
- **Restaurant & Menu:** Captures restaurants, their details, menus, and menu items.
- **Cart & Order Management:** Supports customer carts, orders, order items, and order statuses.
- **Payment System:** Integrates multiple payment methods, transaction records, configurations, and statuses.
- **Auditing:** Logs user actions and changes across the system for tracking and accountability.

This ERD provides a full overview of the entities, relationships, and structure needed for a scalable and production-ready food delivery platform.


![Food Delivery DB Design](https://github.com/ABDULLAH1SAID/restaurant-ordering-system-design/blob/main/FoodDelivery/FOOD_DELIVERYERD.png)

--- 
### API Signature

## 👤 User Registration & Authentication API

| Operation        | Endpoint                  | Input                     | Output Status Code | Description |
|------------------|---------------------------|---------------------------|------------------|------------|
| Register         | `/api/v1/user/register`   | user_details {}            | 201              | Creates a new user account. |
| Activate Account | `/api/v1/user/activate`   | activationToken            | 200              | Activates the user account via email verification. |
| Login            | `/api/v1/user/login`      | username / email, password | 200 + Token      | Authenticates the user and returns an access token. |
| Logout           | `/api/v1/user/logout`     | customerId                 | 200              | Logs out the user and invalidates the token. |
| Reset Password   | `/api/v1/user/forgetpass` | email                      | 200              | Sends a password reset link or code to the user’s email. |

## 🧑 User Profile Management API

| Operation        | Endpoint                    | Input                          | Output Status Code | Description |
|------------------|-----------------------------|--------------------------------|------------------|------------|
| Get Profile      | `/api/v1/user/profile`      | customerId                     | 200              | Retrieves user profile information. |
| Update Profile   | `/api/v1/user/update`       | customerId, user_details {}    | 200              | Updates user profile information. |
| Change Password  | `/api/v1/user/change-pass`  | customerId, oldPassword, newPassword | 200        | Changes the user password. |
| Delete Account   | `/api/v1/user/delete`       | customerId                     | 200              | Deletes the user account. |

## 🛒 Cart Management API

| Operation      | Endpoint                 | Input                                      | Output Status Code | Description |
|----------------|-------------------------|-------------------------------------------|------------------|------------|
| Add to Cart    | `/api/v1/cart/add`       | cartItem, customerId                       | 201              | Adds a new item to the specified customer's cart. |
| Clear Cart     | `/api/v1/cart/clear`     | cartId, customerId                         | 200              | Removes all items from the specified cart. |
| Remove Item    | `/api/v1/cart/remove-item` | cartId, customerId, itemId                | 200              | Removes a specific item from the cart. |
| Modify Cart    | `/api/v1/cart/modify`    | cartId, customerId, itemId, quantity      | 200              | Updates the quantity of an existing item in the cart. |

## 📦 Order Management API

| Operation        | Endpoint                    | Input                                   | Output Status Code | Description |
|------------------|-----------------------------|------------------------------------------|------------------|------------|
| Create Order     | `/api/v1/order/create`      | cartId, customerId, addressId, paymentMethod | 201              | Creates a new order from the user's cart. |
| Get Order By ID  | `/api/v1/order/{orderId}`   | orderId, customerId                      | 200              | Retrieves order details by order ID. |
| Get User Orders  | `/api/v1/order/my-orders`   | customerId                               | 200              | Retrieves all orders for the logged-in user. |
| Cancel Order     | `/api/v1/order/cancel`      | orderId, customerId                      | 200              | Cancels an order if it is still pending. |
| Update Order Status | `/api/v1/order/update-status` | orderId, status                        | 200              | Updates the order status (Admin only). |

## 🍽️ Restaurant & Menu Management API

| Operation              | Endpoint                              | Input                                      | Output Status Code | Description |
|------------------------|---------------------------------------|--------------------------------------------|------------------|------------|
| Create Restaurant      | `/api/v1/restaurant/create`           | restaurant_details {}                       | 201              | Creates a new restaurant. |
| Update Restaurant      | `/api/v1/restaurant/update`           | restaurantId, restaurant_details {}         | 200              | Updates restaurant information. |
| Get Restaurants        | `/api/v1/restaurant/list`             | —                                          | 200              | Retrieves all restaurants. |
| Get Restaurant By ID   | `/api/v1/restaurant/{restaurantId}`   | restaurantId                               | 200              | Retrieves restaurant details. |
| Delete Restaurant      | `/api/v1/restaurant/delete`           | restaurantId                               | 200              | Deletes a restaurant (Admin only). |
| Create Menu Item       | `/api/v1/menu/create`                 | restaurantId, menu_item_details {}          | 201              | Adds a new menu item to a restaurant. |
| Update Menu Item       | `/api/v1/menu/update`                 | menuItemId, menu_item_details {}            | 200              | Updates menu item details. |
| Delete Menu Item       | `/api/v1/menu/delete`                 | menuItemId                                 | 200              | Deletes a menu item. |
| Get Menu By Restaurant | `/api/v1/menu/{restaurantId}`         | restaurantId                               | 200              | Retrieves menu items for a restaurant. |

## 💳 Payment Integration API

| Operation          | Endpoint                      | Input                                   | Output Status Code | Description |
|--------------------|-------------------------------|------------------------------------------|------------------|------------|
| Create Payment     | `/api/v1/payment/create`      | orderId, paymentMethod                   | 201              | Initiates a payment for an order. |
| Process Payment    | `/api/v1/payment/process`     | paymentId, paymentDetails {}             | 200              | Processes the payment transaction. |
| Verify Payment     | `/api/v1/payment/verify`      | paymentId                                | 200              | Verifies payment status with the payment gateway. |
| Payment Callback   | `/api/v1/payment/callback`    | transactionData {}                       | 200              | Handles payment gateway callbacks/webhooks. |
| Refund Payment     | `/api/v1/payment/refund`      | paymentId, reason                        | 200              | Refunds a completed payment. |








