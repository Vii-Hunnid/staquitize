import QRCode from 'qrcode';

// In the template
<template>
  <!-- Add an element to display the QR code -->
  <div id="qr-code"></div>
</template>

// In the Vue component
export default {
  // Other code
  created() {
    // Encode the banking details as a string
    const bankingDetailsString = JSON.stringify(this.bankingDetails);

    // Generate the QR code
    QRCode.toElement(bankingDetailsString, { element: document.getElementById('qr-code') });
  },
};
