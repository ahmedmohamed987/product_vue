<template>
  <div class="container mt-5">
    <h2 class="text-center mb-4">Edit Product</h2>

    <div v-if="message" class="alert" :class="messageType">
      {{ message }}
    </div>

    <form @submit.prevent="updateProduct">
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

      <button type="submit" class="btn btn-primary mt-3">Update Product</button>
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
  created() {
    this.fetchProduct();
  },
  methods: {
    async fetchProduct() {
  const productId = this.$route.params.id; 
  try {
    const response = await axios.get(`/showproducts/${productId}`, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('token')}`, 
      }
    });

    this.product = response.data;
  } catch (error) {
    console.error('Error fetching product details:', error.response);
    this.message = 'Error fetching product details.';
    this.messageType = 'alert-danger';
  }
},


    async updateProduct() {
  const productId = this.$route.params.id; 
  try {
    const response = await axios.put(`/products/${productId}`, this.product, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('token')}`, 
      }
    });

    this.message = 'Product updated successfully!';
    this.messageType = 'alert-success'; 

    this.$router.push({ name: 'index' }); 

  } catch (error) {
    this.message = 'Error updating product. Please try again.';
    this.messageType = 'alert-danger'; 
    console.error('Error:', error.response);
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
