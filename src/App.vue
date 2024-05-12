<template>
  <div class="app">
    <h1>Rupiah to Dollar Converter</h1>
    <label for="rupiahAmount">Amount in Rupiah:</label>
    <input type="number" id="rupiahAmount" v-model="rupiahAmount">
    <br>
    <label for="exchangeRate">Exchange Rate (1 USD to IDR):</label>
    <input type="number" id="exchangeRate" v-model="exchangeRate">
    <br>
    <p v-if="exchangeRate !== 0">Current Exchange Rate: {{ exchangeRate }}</p>
    <p>Amount in Dollar: {{ convertToDollar }}</p>
    <button class="custom-button" @click="convert">Convert</button>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue';

export default {
  name: 'App',
  setup() {
    const rupiahAmount = ref(0);
    const exchangeRate = ref(0);

    const fetchExchangeRate = async () => {
      try {
        const response = await fetch('https://api.exchangerate-api.com/v4/latest/USD');
        const data = await response.json();
        exchangeRate.value = data.rates.IDR;
      } catch (error) {
        console.error('Failed to fetch exchange rate:', error);
      }
    };

    onMounted(fetchExchangeRate);

    const convertToDollar = computed(() => {
      const amountInRupiah = parseFloat(rupiahAmount.value);
      const rate = parseFloat(exchangeRate.value);
      const amountInDollar = amountInRupiah / rate;
      return isNaN(amountInDollar) ? '' : amountInDollar.toFixed(2);
    });

    const convert = () => {
      // Logic to convert amount
    };

    return {
      rupiahAmount,
      exchangeRate,
      convertToDollar,
      convert
    };
  }
}
</script>

<style>
body {
  font-family: 'Space Mono', monospace;
}
.app {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Space Mono', monospace;
}
label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}
input {
  margin-bottom: 10px;
  padding: 5px;
  width: 100%;
}
p {
  margin-top: 20px;
}
.custom-button {
  background-color: #4CAF50;
  border: none;
  color: white;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s;
}
.custom-button:hover {
  background-color: #45a049;
}
</style>
