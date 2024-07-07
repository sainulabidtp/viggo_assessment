<template>
  <div id="app">
    <!-- ProductsList component with enhanced design -->
    <ProductsList :products="products" @add-to-cart="addToCart" />
    
    <!-- ShoppingCart component with enhanced design -->
    <ShoppingCart
      :cartItems="cartItems"
      @load-cart="loadCart"
      @update-cart="updateCart"
      @remove-item="removeCartItem"
    />
    
    <!-- Notification for added items with improved styling -->
    <div v-if="showNotification" class="notification">
      {{ notificationMessage }}
    </div>
  </div>
</template>

<script>
import ShoppingCart from './components/ShoppingCart.vue';
import ProductsList from './components/ProductsList.vue';

export default {
  name: 'App',
  components: {
    ShoppingCart,
    ProductsList,
  },
  data() {
    return {
      // Sample products list
      products: [
        { id: 1, name: 'Product 1', price: 10.0 },
        { id: 2, name: 'Product 2', price: 20.0 },
        { id: 3, name: 'Product 3', price: 15.0 },
      ],
      // Array to hold items in the shopping cart
      cartItems: [],
      // Notification state
      showNotification: false,
      notificationMessage: '',
    };
  },
  methods: {
    /**
     * Add a product to the shopping cart.
     * @param {Object} product The product object to add.
     */
    addToCart(product) {
      const existingItem = this.cartItems.find((item) => item.id === product.id);
      if (existingItem) {
        // If item already exists in cart, increase quantity
        existingItem.quantity++;
        existingItem.totalPrice = existingItem.quantity * existingItem.price;
      } else {
        // Otherwise, add new item to cart with initial quantity and total price
        this.cartItems.push({
          ...product,
          quantity: 1,
          totalPrice: product.price,
        });
      }

      // Show notification message
      this.showNotification = true;
      this.notificationMessage = `${product.name} added to cart`;

      // Hide notification after 2 seconds
      setTimeout(() => {
        this.showNotification = false;
        this.notificationMessage = '';
      }, 2000);
    },

    /**
     * Load saved cart items from localStorage.
     * @param {Array} savedCartItems Array of cart items from localStorage.
     */
    loadCart(savedCartItems) {
      this.cartItems = savedCartItems;
    },

    /**
     * Update the cart with new items.
     * @param {Array} newCartItems Updated array of cart items.
     */
    updateCart(newCartItems) {
      this.cartItems = [...newCartItems];
    },

    /**
     * Remove an item from the cart.
     * @param {Object} item The item object to remove.
     */
    removeCartItem(item) {
      this.cartItems = this.cartItems.filter((cartItem) => cartItem.id !== item.id);
    },
  },
};
</script>

<style>
/* Global styles for enhanced design */

body {
  font-family: 'Roboto', Arial, sans-serif;
  background-color: #f4f4f4;
  color: #333;
}

/* Improved layout and styling for notification */
.notification {
  position: fixed;
  top: 20px;
  right: 20px;
  background-color: #4caf50; /* Green background color */
  color: white; /* White text color */
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1); /* Subtle box shadow */
  z-index: 999; /* Ensure it's above other content */
}

/* Improved layout and styling for app container */
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh; /* Full viewport height */
  padding: 20px;
}

/* Responsive design for mobile devices */
@media (max-width: 768px) {
  #app {
    padding: 10px;
  }
}
</style>
