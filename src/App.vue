<template>
  <div>
    <div class="invoice">
      <div class="invoice-header">
        <div class="invoice-logo">
          <img src="./assets/logo2.png" alt="Company logo">
        </div>
        <div class="invoice-info">
          <h1>Invoice</h1>
            <p>Invoice number: #{{ invoiceNumber }}</p>
            <h5>Date sent: {{ currentDate }}</h5>
          <div class="company-addresses">
            <div class="invoice-issue-date">
              <h5>Issue date: {{ pastDate }} <br /> </h5>
              <h4>From</h4>
              <p>
                123 Godly St. Internet, Gauteng, South Africa
              </p>
              <h4>Company number</h4>
            </div>  
            <div class="invoice-due-date">  
              <h5>Due date: {{ futureDate }} <br /> </h5>
              <!-- futureDate: new Date(Date.parse('2025-01-01')) -->
              <h4>To</h4> 
              <p>
                123 Godly St. Internet, Gauteng, South Africa
              </p>     
              <h4>VAT number</h4>      
            </div>
          </div>  
        </div>
      </div>
      <div class="invoice-content">
        <table>
          <thead>
            <tr>
              <th>Product</th>
              <th>Description</th>
              <th>Quantity</th>
              <th>Price</th>
              <th>Total</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(product) in products" :key="product.name">
              <td><input maxlength="150" v-model="product.name" /></td>
              <td><input maxlength="250" v-model="product.name" /></td>
              <td><input type="number" v-model.number="product.quantity" /></td>
              <td><input type="number" v-model.number="product.price" /></td>
              <td>{{ product.total }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="invoice-bottom-content">

        <div class="invoice-banking-details">
          <h3>PAYMENT DETAILS:</h3>   
          <img src="./assets/bankingQRCode.png" alt="Company Banking QR-Code">
          <h4>Bank Transfer</h4>      
          <div class="main flex-item">Account Holder: Your Company Name (PTY) LTD<br />
            Account Type: Your Bank Name<br />
            Account Number: 1234567891011<br />
            Branch Code: 0123456<br />
            BIC/SWIFT: ABCDEGFH<br />
            <h5>Paypal: yourpersonal@email.com</h5>
          </div>
        </div>

        <div class="invoice-totals">
          <p>Subtotal: R {{ subtotal }}</p>
          <p>Tax: R {{ tax }}</p>
          <p>Vat: R {{ vat }}</p>
          <p>Discount: R {{ discount }}</p>
          <h3>Total: R {{ total }}</h3>
        </div>
      </div>

      <div class="invoice-footer">
        <h3>Terms & conditions</h3>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. 
          Ut enim ad minim veniam, quis nostrud.
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. 
          Ut enim ad minim veniam, quis nostrud.
        </p>
        <h5>Thank you for your business!</h5>
      </div>
    </div>
    <div class="editing-buttons">
      <button @click="addProduct">Add Product</button>
      <button @click="deleteProduct(index)">Delete</button>
      <button @click="printInvoice">Print</button>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      currentDate: new Date().toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'short',
        day: 'numeric'
      }),
      pastDate: new Date(2020, 0, 1).toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'short',
        day: 'numeric'
      }),
      futureDate: new Date(2025, 0, 1).toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'short',
        day: 'numeric'
      }),

      products: [
        { name: 'Product 1', quantity: 2, price: 50.00 },
        { name: 'Product 2', quantity: 3, price: 40.00 },
        { name: 'Product 3', quantity: 1, price: 60.00 }
      ],
      taxRate: 0.15, // 15% tax rate
      discountRate: 0.1, // 10% discount rate
    }
  },
  computed: {
    subtotal() {
      return this.products.reduce((total, product) => total + product.total, 0.00);
    },
    tax() {
      return this.subtotal * this.taxRate;
    },    
    vat() {
      return this.subtotal * this.vatRate;
    },
    discount() {
      return this.subtotal * this.discountRate;
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
      this.products.push({ name: '', quantity: 0, price: 0.00 });
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
  /* Add your own styles here */
  .invoice {
    width: 800px;
    margin: 0 auto;
    border: 1px solid #ccc;
    border-radius: 10px;
    font-family: Arial, sans-serif;
    position: relative;
  }
  
  .invoice-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px;
    border-bottom: 1px solid #ccc;
  }

  .invoice-logo {
    /* position: absolute; */
    /* background-color: #000;
    height: 100%; */

  }
  
  .invoice-logo img {
    width: 100px;
    align-items: top;
  }
  
  .invoice-info {
    text-align: right;
  }

  /* .company-addresses, h5, h4, h3, h2, p {
    margin-bottom: -21.440px;
  } */
  
  .invoice-content {
    padding: 5px;
    display: grid;
    place-items: start;
  }

  .company-addresses{
    width: 360px;
    height: 250px;
    box-sizing: border-box;
    display: flex;
    gap: 16.1%;
  }

  .invoice-issue-date{
    width: 150px;
    height: 250px;
    text-align: left;

    text-transform: capitalize;
    display: flex;
    justify-content: start;
    flex-direction: column;
    flex-flow: wrap;
    flex-wrap: wrap;
  }

  .invoice-due-date{
    width: 150px;
    height: 250px;
    text-align: left;

    text-transform: capitalize;
    display: flex;
    justify-content: start;
    flex-direction: column;
    flex-flow: wrap;
    flex-wrap: wrap;
  }

  .invoice-bottom-content{
    width: 800px;
    /* height: 350px; */
    box-sizing: border-box;
    border-top: 1px solid #ccc;
    display: flex;
    gap: 10%;
  }

  .invoice-banking-details{
    width: 100%;
    display: grid;
    place-items: start;

    padding: 20px;
  }

  .invoice-totals {
    width: 100%;
    display: grid;
    place-items: end;

    padding: 20px;
  }
  
  .invoice-footer {
    padding: 20px;
    text-align: start;
    font-size: 10px #cccccc;
    border-top: 1px solid #ccc;
  }

  .editing-buttons{
    display: flex;
    justify-content: space-evenly;
    padding: 20px 0;
    border-top: 1px solid #ccc;
  }
  
  /* Print styles */
  @media print {
    button {
    display: none;
    }
    .invoice {
      width: 100%;
      border: none;
    }

      table, th, td {
      border: 1px solid rgb(255, 255, 255);
      padding: 5px;
    }
  }

  table {
    max-width: 400px;
    width: 100% !important;
    border-collapse: collapse;
  }
  
  table, th, td {
    border: 1px solid #000;
    padding: 5px;
  }

  table, input {
    border: 1px solid rgb(255, 255, 255);
  }

  </style>
