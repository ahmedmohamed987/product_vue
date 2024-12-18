<template>
    <div class="container mt-5">
      <h2>Product List</h2>
      <table v-if="products.length > 0" class="table table-striped">
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Price</th>
            <th>Quantity</th>
            <th>Operations</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="product in products" :key="product.id">
            <td>{{ product.id }}</td>
            <td>{{ product.name }}</td>
            <td>${{ product.price }}</td>
            <td>{{ product.quantity }}</td>
            <td>
              <router-link :to="`/edit/products/${product.id}`" class="btn btn-secondary m-1" role="button">Edit</router-link>
              <button @click.prevent="confirmDelete(product.id)" class="btn btn-danger m-1">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
  
      <p v-else>No products available.</p>
  
      <div>
        <router-link to="/create/product" class="btn btn-primary" role="button">Add New Product</router-link>
      </div>
    </div>
  </template>
  
  <script>
  import axios from "../axios";
  
  export default {
    data() {
      return {
        products: [], 
      };
    },
    created() {
      this.fetchProducts(); 
    },
    methods: {
      async fetchProducts() {
        try {
          const response = await axios.get("/products", {
            headers: {
              Authorization: `Bearer ${localStorage.getItem('token')}`,
            },
          });
          this.products = response.data; 
        } catch (error) {
          console.error("Error fetching products:", error);
        }
      },
  
     
      async confirmDelete(productId) {
        if (confirm("Are you sure you want to delete this product?")) {
          this.deleteProduct(productId);
        }
      },
  
      // Delete a product from the backend
      async deleteProduct(productId) {
        try {
          await axios.delete(`/products/${productId}`, {
            headers: {
              Authorization: `Bearer ${localStorage.getItem('token')}`,
            },
          });
          this.fetchProducts(); 
        } catch (error) {
          console.error("Error deleting product:", error.response);
        }
      },
    },
  };
  </script>
  