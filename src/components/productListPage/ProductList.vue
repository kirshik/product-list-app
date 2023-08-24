<template>
  <div class="product-list">
    <h1>Product List</h1>
    <div class="input-container">
      <input v-model="newProduct" @keyup.enter="addProduct" placeholder="Enter a new product">
      <button @click="addProduct" class="add-button">Add</button>
    </div>
    <ul class="product-items">
      <li v-for="(product, index) in products" :key="index" class="product-item">
        <span :class="{ 'strikethrough': product.completed }">{{ product.name }}</span>
        <div class="buttons">
          <button @click="toggleProductCompletion(index)" :class="['complete-button' + (product.completed ? ' checked' : '')]">
            {{ product.completed ? 'Uncheck' : 'Check' }}
          </button>
          <button @click="removeProduct(index)" class="remove-button">Remove</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newProduct: '',
      products: []
    };
  },
  created() {
    // Load the list from local storage when the component is created
    const savedProducts = localStorage.getItem('products');
    if (savedProducts) {
      this.products = JSON.parse(savedProducts);
    }
  },
  methods: {
    addProduct() {
      if (this.newProduct.trim() !== '') {
        this.products.unshift({ name: this.newProduct, completed: false });
        this.newProduct = '';



        this.saveProductsToLocalStorage();
      }
    },
    toggleProductCompletion(index) {




      this.products[index].completed = !this.products[index].completed;
      if (this.products[index].completed) {
        this.products.push(this.products.splice(index, 1)[0]);
      } else{
        this.products.unshift(this.products.splice(index, 1)[0]);
      }
      this.saveProductsToLocalStorage();
    },
    removeProduct(index) {
      this.products.splice(index, 1);
      this.saveProductsToLocalStorage();
    },
    saveProductsToLocalStorage() {
      localStorage.setItem('products', JSON.stringify(this.products));
    }
  }
};
</script>

<style scoped>
 @import '@/assets/css/productList.css';
</style>
