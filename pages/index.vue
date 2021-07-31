<template>
  <div class="container">
    <figure>
      <b-img
        src="https://image.flaticon.com/icons/png/512/3310/3310608.png"
        alt=""
      />
    </figure>
    <div class="section-currency">
      <h2 class="title">โปรแกรมแปลงสกุลเงิน</h2>

      <div class="currency">
        <b-form-select v-model="currency_1" :options="options"></b-form-select>
        <b-form-input
          v-model="input1"
          type="number"
          @keyup="calculateExchangeRate()"
          min="1"
        ></b-form-input>
      </div>

      <div class="hint">1 {{ currency_1 }} = {{ result }} {{ currency_2 }}</div>

      <div class="group-btn">
        <b-button variant="light" @click="swopCurrency()"
          >สลับสกุลเงิน</b-button
        >
      </div>

      <div class="currency">
        <b-form-select v-model="currency_2" :options="options"></b-form-select>
        <b-form-input
          v-model="input2"
          type="number"
          @keyup="calculateExchangeRate()"
        ></b-form-input>
      </div>
      <!-- 
      <div class="hint">1 {{ currency_2 }} = {{ result }} {{ currency_1 }}</div> -->
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      input1: 1,
      currency_1: 'THB',
      input2: 0,
      currency_2: 'USD',
      options: [
        { value: 'THB', text: 'THB' },
        { value: 'USD', text: 'USD' },
      ],
      result: 0,
    }
  },
  created() {
    this.calculateExchangeRate()
  },
  methods: {
    async calculateExchangeRate() {
      try {
        const response = await axios.get(
          'https://api.exchangerate-api.com/v4/latest/' + this.currency_1
        )

        this.result = response.data.rates[this.currency_2].toFixed(2) * 1
        this.input2 = (this.input1 * this.result)
      } catch (error) {}
    },
    swopCurrency() {
      const _input1 = this.input1;
      this.input1 = Number(this.input2);
      this.input2 = _input1;

      const _sc1 = this.currency_1
      const _sc2 = this.currency_2
      this.currency_1 = _sc2
      this.currency_2 = _sc1

      this.calculateExchangeRate()
    },
  },
}
</script>
<style scoped>
figure {
  display: flex;
  justify-content: center;
}
img {
  margin: 10px auto;
  width: 100%;
  max-width: 250px;
}
.title {
  font-weight: bold;
  text-align: center;
  margin: 20px 0;
}
.section-currency {
  max-width: 400px;
  width: 100%;
  margin: 10px auto;
}
.group-btn {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}
.currency {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-gap: 10px;
  margin-top: 20px;
}
.currency option {
  outline: unset;
  font-size: 20px;
  padding: 20px 10px;
  border-radius: 5px;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.currency input[type='number'] {
  text-align: right;
  font-size: 20px;
  padding: 10px;
  -webkit-appearance: none;
  -moz-appearance: textfield;
}
.hint {
  text-align: right;
}
</style>
