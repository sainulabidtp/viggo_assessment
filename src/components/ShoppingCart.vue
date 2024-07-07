<template>
  <div class="shopping-cart">
    <h2>Shopping Cart</h2>
    
    <!-- Display message when cart is empty -->
    <div v-if="cartItems.length === 0" class="empty-cart">
      <p>Your cart is currently empty. Start adding items to see them here!</p>
    </div>
    
    <!-- Display cart items -->
    <div v-else>
      <!-- Loop through cart items -->
      <div v-for="item in cartItems" :key="item.id" class="cart-item">
        <!-- Item information -->
        <div class="item-info">
          <div>{{ item.name }}</div>
          <div>Price: ${{ item.price.toFixed(2) }}</div>
          <div>
            Quantity:
            <button @click="decreaseQuantity(item)">-</button>
            {{ item.quantity }}
            <button @click="increaseQuantity(item)">+</button>
          </div>
        </div>
        <!-- Total price for the item -->
        <div class="item-total">
          Total Price: ${{ item.totalPrice.toFixed(2) }}
        </div>
        <!-- Button to remove item from cart -->
        <div class="button-container">
          <button @click="removeItem(item)" class="remove-button">Remove</button>
        </div>
      </div>
      
      <!-- Cart summary section -->
      <div class="cart-summary">
        <!-- Discount code input and apply button -->
        <div class="discount">
          <input type="text" v-model="discountCode" placeholder="Discount Code" />
          <button @click="applyDiscount">Apply</button>
        </div>
        
        <!-- Display applied discount amount -->
        <div v-if="discountAmount > 0" class="discount-amount">
          Discount: -${{ discountAmount.toFixed(2) }}
        </div>
        
        <!-- Total price of all items in the cart -->
        <div class="total">
          Total: ${{ cartTotal.toFixed(2) }}
        </div>
        
        <!-- Button to proceed to checkout -->
        <button @click="checkout">Proceed to Checkout</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ShoppingCart',
  props: ['cartItems'], // Receive cart items as a prop

  data() {
    return {
      discountCode: '', // Stores user input for discount code
      discountAmount: 0, // Stores the calculated discount amount
    };
  },

  computed: {
    // Calculate subtotal of all items in the cart
    cartSubtotal() {
      return this.cartItems.reduce((total, item) => total + item.totalPrice, 0);
    },
    
    // Calculate total price after applying discount
    cartTotal() {
      return this.cartSubtotal - this.discountAmount;
    },
  },

  created() {
    // Load saved cart items from localStorage when component is created
    const savedCartItems = localStorage.getItem('cartItems');
    if (savedCartItems) {
      this.$emit('load-cart', JSON.parse(savedCartItems));
    }
  },

  watch: {
    // Watch for changes in cartItems and update localStorage
    cartItems: {
      handler(newCartItems) {
        localStorage.setItem('cartItems', JSON.stringify(newCartItems));
      },
      deep: true,
    },
  },

  methods: {
    // Increase quantity of a specific item in the cart
    increaseQuantity(item) {
      item.quantity++;
      item.totalPrice = item.quantity * item.price;
      this.$emit('update-cart', this.cartItems); // Emit event to update parent component
    },
    
    // Decrease quantity of a specific item in the cart
    decreaseQuantity(item) {
      if (item.quantity > 1) {
        item.quantity--;
        item.totalPrice = item.quantity * item.price;
        this.$emit('update-cart', this.cartItems); // Emit event to update parent component
      }
    },
    
    // Remove a specific item from the cart
    removeItem(item) {
      this.$emit('remove-item', item); // Emit event to remove item from parent component
    },
    
    // Apply discount based on user input
    applyDiscount() {
      if (this.discountCode === 'SAVE10') {
        this.discountAmount = this.cartSubtotal * 0.1; // Apply 10% discount
      } else if (this.discountCode === 'SAVE20') {
        this.discountAmount = this.cartSubtotal * 0.2; // Apply 20% discount
      } else {
        this.discountAmount = 0; // Reset discount amount if invalid code
        alert('Invalid discount code');
      }
    },
    
    // Placeholder method for checkout functionality
    checkout() {
      alert('Redirect to checkout page or perform checkout action');
    },
  },
};
</script>

<style scoped>
/* Scoped styles for ShoppingCart component */

.shopping-cart {
  font-family: 'Arial', sans-serif;
  max-width: 800px;
  margin: auto;
  padding: 20px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.empty-cart {
  font-style: italic;
  text-align: center;
  margin-top: 20px;
  background-color: #f8f8f8;
  padding: 20px;
  border-radius: 5px;
  border: 1px solid #ddd;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  margin-bottom: 10px; /* Added margin-bottom for space */
  border: 1px solid #ddd;
  border-radius: 5px;
  transition: all 0.3s ease;
}

.item-info {
  flex: 1;
}

.item-total {
  text-align: right;
}

.button-container {
  display: flex;
  justify-content: flex-end;
}

.remove-button {
  background-color: #ff6347;
  color: white;
  border: none;
  padding: 2px 16px; /* Added padding */
  border-radius: 3px;
  cursor: pointer;
  margin-left: 20px;
  transition: background-color 0.3s ease;
}

.remove-button:hover {
  background-color: #ff483b;
}

.cart-summary {
  margin-top: 20px;
  padding: 10px;
  background-color: #f8f8f8;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.discount {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.discount input {
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 3px;
  margin-right: 10px;
}

.discount button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 8px 16px; /* Adjusted padding */
  border-radius: 3px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.discount button:hover {
  background-color: #45a049;
}

.discount-amount {
  color: red;
  margin-bottom: 10px;
}

.total {
  font-weight: bold;
  margin-bottom: 10px;
}

button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 3px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #45a049;
}

@media (max-width: 600px) {
  .cart-item {
    flex-direction: column;
    align-items: flex-start;
  }

  .item-info {
    margin-bottom: 10px;
  }

  .cart-summary {
    padding: 10px;
  }
}
</style>
