<template>
  <div class="currency-widget">
    <div class="header">
      <h2>Курсы валют</h2>
    </div>
    <div class="content">
      <div v-if="loading" class="loading">Загрузка...</div>
      <div v-else>
        <ul class="rates-list">
          <li v-for="(rate, currency) in rates" :key="currency" class="rate-item">
            <span class="currency"><span class="flag-icon flag-icon-{{ currency.toLowerCase() }}"></span> {{ currency }}:</span>
            <span class="rate">{{ rate.toFixed(2) }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>



<script>
import axios from 'axios';

export default {
  name: 'CurrencyWidget',
  data() {
    return {
      rates: {},
      loading: true
      //limit: 5 // Ограничиваем количество выводимых валют
    };
  },
  computed: {
    limitedRates() {  
      return Object.fromEntries(Object.entries(this.rates).slice(0, this.limit));
    }
  },
   async created() {
    try {
      const response = await axios.get('https://api.exchangerate-api.com/v4/latest/USD');
      this.rates = response.data.rates;
      this.loading = false;
    } catch (error) {
      console.error('Ошибка при загрузке курсов валют:', error);
    }
  },

};
</script>

<style scoped>
@import '../assets/styles.css';
</style>