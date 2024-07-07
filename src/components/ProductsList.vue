<template>
  <div>
    <h2>Products</h2>
    <div class="product-list">
      <!-- Loop through products and display each product item -->
      <div v-for="product in products" :key="product.id" class="product-item">
        <div>{{ product.name }}</div>
        <div>Price: ${{ product.price.toFixed(2) }}</div>
        <!-- Button to add product to cart -->
        <button @click="addToCart(product)">Add to Cart</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductsList',
  props: ['products'], // Receive products array as a prop

  methods: {
    /**
     * Emit event to add a product to the cart.
     * @param {Object} product The product object to add to the cart.
     */
    addToCart(product) {
      this.$emit('add-to-cart', product); // Emit event with selected product
    }
  }
};
</script>

<style scoped>
/* Scoped styles for ProductsList component */

.product-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.product-item {
  width: calc(33.33% - 20px); /* Adjust width as per your design */
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
  transition: all 0.3s ease; /* Smooth transition on hover */
}

.product-item:hover {
  background-color: #f9f9f9; /* Light background color on hover */
}

button {
  background-color: #4CAF50; /* Green background color */
  color: white; /* White text color */
  border: none;
  padding: 10px 20px;
  border-radius: 3px;
  cursor: pointer;
  transition: background-color 0.3s ease; /* Smooth background transition */
}

button:hover {
  background-color: #45A049; /* Darker green on hover */
}
</style>
