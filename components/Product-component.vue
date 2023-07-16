<template>
  <div>
    <div v-for="product in products" :key="product.id">
      <h3>{{ product.name }}</h3>
      <p>Quantity: {{ product.quantity }}</p>
      <p>Sale Price: {{ product.sale_price }}</p>
      <p>Barcode: {{ product.barcode }}</p>
      <p>Price: {{ product.price }}</p>
      <p>Created At: {{ product.createdAt }}</p>
      <p>Product Type: {{ product.product_type_name }}</p>
      <p>Unit Name: {{ product.unit_name }}</p>
      <p>Supplier Name: {{ product.supplier_name }}</p>
      <p>Address: {{ product.address }}</p>
      <p>Tel: {{ product.tel }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: []
    };
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    fetchProducts() {
      fetch('http://localhost:2023/Product')
        .then(response => response.json())
        .then(data => {
          this.products = data;
        })
        .catch(error => {
          this.$toast.success('Error fetching products:', error);
        });
    }
  }
};
</script>

<style scoped>
/* Add any necessary styles here */
</style>
