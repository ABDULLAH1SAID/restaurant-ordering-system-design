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

This section presents the **analysis and design of the Cart module**, covering both the **data layer** and **behavioral aspects** of the system.

It includes:
- Database design and data modeling
- Control flow and system interactions
- Core business logic for cart operations

The analysis focuses on the following core use cases:
- Add to Cart  
- Modify Cart  

The goal is to provide a clear understanding of how the cart is structured, how data flows through the system, and how different components interact to support real-world e-commerce scenarios.

--- 

## 🗄️ Database Design (Cart Module)

This section describes the database design for the **Cart module** in the restaurant ordering system.  
The design focuses on managing user carts, cart items, and their relationship with menu items in a scalable and realistic e-commerce scenario.

### 📌 Entity Relationship Diagram (ERD)

![Cart ERD](https://github.com/ABDULLAH1SAID/restaurant-ordering-system-design/blob/main/FoodDelivery/ERD.png)

---

### 🛒 CART

Represents the shopping cart entity that groups selected menu items before checkout.

**Attributes:**
- `cart_id` (PK): Unique identifier for the cart
- `total_price`: Cached total price of all items in the cart
- `status`: Cart state (`ACTIVE`, `CHECKED_OUT`, `ABANDONED`)
- `created_at`: Cart creation timestamp
- `updated_at`: Last update timestamp

---

### 📦 CART_ITEM

Acts as a junction entity between **Cart** and **Menu Item**, representing items added to the cart.

**Attributes:**
- `cart_item_id` (PK): Unique identifier for the cart item
- `cart_id` (FK): Reference to the associated cart
- `item_id` (FK): Reference to the menu item
- `quantity`: Number of units added
- `unit_price`: Item price at the time of addition (price snapshot)

---

### 🍔 MENU_ITEM

Represents items available on restaurant menus.

**Attributes:**
- `item_id` (PK): Unique identifier for the menu item
- `name`: Item name
- `description`: Item description
- `category`: Item category
- `price`: Current item price

---

### 🔗 Relationships

- One **Cart** can contain multiple **Cart Items** (1 → N)
- Each **Cart Item** belongs to one **Cart**
- Each **Cart Item** refers to one **Menu Item**
- A **Menu Item** can exist in multiple carts

---

### 🧠 Design Decisions

- The cart is treated as a standalone entity to support historical carts and future scalability.
- `CART_ITEM` stores a snapshot of the item price (`unit_price`) to prevent inconsistencies if menu prices change.
- Only one active cart exists per user at a time, while previous carts are preserved for history and order creation.
- The cart total price is stored and recalculated on every cart modification to improve performance.
- The design follows normalization principles while remaining optimized for real-world e-commerce systems.

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
📲 [Erd](https://mermaid.live/edit#pako:eNq1GNluqzj0V5Cf26pps5W3KKFVNG1SEfJwR5WQA05qXbAZ27STafvv97CEspgJmdzJS-Cc47Nv5gN53CfIRETMKN4JHL6wF2bAb72ybOMje05-lCmD-sbzH98gqQRlO2MbB4HLcEgaGBJiGjSgEZbynQu_iXjlrMRkw3lAMDOodLGn6FsJ5WNFFA2J4QkCj76LlQYZR34F-XWwzV4-Wt1s05rlE-kJGinKWY1x4jS3A_cEFEsiXIDf1-ACrC7DC-bT9cpZPh2NShvvQvktjgPlYt8XRMoG3qNqX2UW8D0O1N6NOLzJDkEoNJ7MZra1WnVQ2Iul4mGr0vBHiDqiaw7cxDTw4cFlcbghopmuAedNKOOKSG3u5Q47xW4w2pms7cnCOSPLatWgL6mSnjwi7PdVSGGBO7OcyfyxSxAhnxSOBWaqJYzdcq6tykq4xNYkxIkRTW4Bl1VkYdaTtVj_DkOqAWtpVBWp7tyxnjqIDgmLOwmFNMbKiAT1WjR5g0TBm6CuzHRiO2cUZCf3AFrFJ3WKRKuuLvKwUDq1MtcpEuqQf8WgbpFmhdylPYOODbnurFen1mqVyf_m0wSbObSOyVJAcYUDt5YIxz2eWd7R5TCt9Wqf4HNt0hbqPE9-QJlADiwc68GeOPPlwnV-PP-nMX2sHnWypsvF_fxhnb118EiE92A7BIspAmtT0qVctY9IS0V4nG3pzv1J9m2oNxzEehsI88Q-giDWzbCte8u2rZl7MGhlOc588XBmKnY17bwZWeh8Xu05MKNWk2nHqLXmcVeby7T_VpQ45DE7yR8lQ04YuQo6hkySHPTVZ54gWyIghUjbPgSdJ-JMEje5CbRjQxjceKchwO_ugai-_q1nc8d9XD6ct7Fm5jU3obS31C4eCascQZsXDB749To7ZBZ5r2OOhyy9In1-Xl7yj9LmbxqvWJbuGRoCuADRHQOWijd4fX5-L_qmwd-ZrG3_Ob_Dbp2LayyeOS_NIldWsEoPbNMNqSw3BZSQ2cgwk7alMGWycTvJadM9w8ydJ0u7Rwnf5FVdlzSksEImoWgVm81i04gC7B3EZrAywYEbZV4Q-6RNcIU27R257MbqUJMOZen9lNX9IA9IuWGVIlEG58x0HcGEtRhcFUhtE20eTA9sKSPyyJjVHoWi9KElHp3R-eHjc9XMp12cOHKz14ewfa5BWNN2JnUV-N1tTGNHGBFZ3qELtBPUR6YSMblAMPPgBgWvKO1JL0i9EmggyITHwxRDL-wLjkWY_cl5eDgpeLx7ReYWBxLesptT_s2kICEMcmSatH5k9oa3KQ9kfqC_kXl5c3191evf9AeD0ehuPBgM-hdoD_Dbu_7VcDy6Hgyv-ze3o5u7rwv0Tyq3dzUeDoa34_GgNxjCqVHvAhGfKi6eso826bebr19_AVSh)







