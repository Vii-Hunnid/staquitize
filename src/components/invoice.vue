<template>
    <div>
      <h1>Invoice</h1>
      <table>
        <thead>
          <tr>
            <th>Product</th>
            <th>Quantity</th>
            <th>Price</th>
            <th>Total</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(product, index) in products" :key="product.name">
            <td><input maxlength="250" v-model="product.name" /></td>
            <td><input type="number" v-model.number="product.quantity" /></td>
            <td><input type="number" v-model.number="product.price" /></td>
            <td>{{ product.total }}</td>
            <td><button @click="deleteProduct(index)">Delete</button></td>
          </tr>
        </tbody>
      </table>
      <p>Subtotal: {{ subtotal }}</p>
      <p>Tax: {{ tax }}</p>
      <p>Vat: {{ vat }}</p>
      <h2>Total: {{ total }}</h2>
      <button @click="addProduct">Add Product</button>
      <button @click="printInvoice">Print</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        products: [
          { name: 'Product 1', quantity: 2, price: 50 },
          { name: 'Product 2', quantity: 3, price: 40 },
          { name: 'Product 3', quantity: 1, price: 60 }
        ],
        taxRate: 0.15, // 15% tax rate
      }
    },
    computed: {
      subtotal() {
        return this.products.reduce((total, product) => total + product.total, 0);
      },
      tax() {
        return this.subtotal * this.taxRate;
      },    
      vat() {
        return this.subtotal * this.vatRate;
      },
      total() {
        return this.subtotal + this.tax;
      },
    },
    created() {
      this.products.forEach(product => {
        product.total = product.quantity * product.price;
      });
    },
    methods: {
      addProduct() {
        this.products.push({ name: '', quantity: 0, price: 0 });
      },
      deleteProduct(index) {
        this.products.splice(index, 1);
      },
      printInvoice() {
        window.print();
      },
    },
    watch: {
      products: {
        handler() {
          this.products.forEach(product => {
            product.total = product.quantity * product.price;
          });
        },
        deep: true,
      },
    },
  };
  
  </script>
  
    
  <style>
    @media print {
    button {
      display: none;
    }
  }
  
    table {
      border-collapse: collapse;
    }
    
    table, th, td {
      border: 1px solid black;
      padding: 8px;
    }
    </style>
    
  