<template>
  <div class="container mt-5">
    <h2 class="text-center mb-4">Create Product</h2>

    <div v-if="message" class="alert" :class="messageType">
      {{ message }}
    </div>

    <form @submit.prevent="createProduct">
      <div class="form-group">
        <label for="name">Product Name</label>
        <input
          type="text"
          id="name"
          v-model="product.name"
          class="form-control"
          required
        />
      </div>

      <div class="form-group mt-3">
        <label for="price">Price</label>
        <input
          type="number"
          id="price"
          v-model="product.price"
          class="form-control"
          required
        />
      </div>

      <div class="form-group mt-3">
        <label for="quantity">Quantity</label>
        <input
          type="number"
          id="quantity"
          v-model="product.quantity"
          class="form-control"
          required
        />
      </div>

      <button type="submit" class="btn btn-primary mt-3">Create Product</button>
    </form>
  </div>
</template>

<script>
import axios from "../axios";

export default {
  data() {
    return {
      product: {
        name: '',
        price: '',
        quantity: ''
      },
      message: null,   
      messageType: null
    };
  },
  methods: {
  async createProduct() {
  try {
    
    const response = await axios.post('/create/product', this.product, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('token')}`, 
      }
    });

    this.message = response.data.message || 'Product created successfully!';
    this.messageType = 'alert-success'; 

    this.$router.push({ name: 'index' });

  } catch (error) {
    if (error.response && error.response.data && error.response.data.message) {
      this.message = error.response.data.message;
    } else {
      this.message = 'Error creating product. Please try again.';
    }

    this.messageType = 'alert-danger'; 

    this.$router.push({ name: 'index' });

    console.error('Error:', error.response || error);
  }
}
}

};
</script>

<style scoped>
.form-group {
  max-width: 500px;
  margin: 0 auto;
}
</style>
