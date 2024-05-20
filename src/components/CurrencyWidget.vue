<template>
  <div class="currency-widget">
    <h2>Курсы валют</h2>
    <div v-if="loading">Загрузка...</div>
    <div v-else>
      <ul>
        <li v-for="(rate, currency) in rates" :key="currency">
          {{ currency }}: {{ rate }}
        </li>
      </ul>
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
      loading: true,
    };
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
.currency-widget {
  border: 1px solid #ccc;
  padding: 10px;
  width: 300px;
}
</style>
