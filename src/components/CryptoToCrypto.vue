<template>
  <div id="fiatStyle">
    <button class="btn-home">
      <router-link to="/">
        <span class="btn-style">Home Page</span>
      </router-link>
    </button>
    <h1>Crypto - Crypto Currency</h1>
    <div class="grid-container">
      <div class="grid-item item1">
        <label>Amount = </label>
        <input
          type="number"
          v-model="amount"
          placeholder="Amount"
          class="input-width"
          required
        />
      </div>
      <div class="grid-item item2">
        <label>From : </label>
        <select v-model="symbol1" class="input-width">
          <option v-for="crypto in cryptoArray">{{ crypto }}</option>
        </select>
        <label>To : </label>
        <select v-model="symbol2" class="input-width">
          <option v-for="crypto in cryptoArray">{{ crypto }}</option>
        </select>
      </div>
      <div class="grid-item item3">
        <p>
          Could not find your pair symbol in dropdown? Type your pair symbol
          below ...
        </p>
      </div>
      <div class="grid-item item4">
        <label>From : </label>
        <input
          type="string"
          v-model="symbol1"
          placeholder="symbol1"
          class="input-width"
        />
        <label>To : </label>
        <input
          type="string"
          v-model="symbol2"
          placeholder="symbol2"
          class="input-width"
        />
      </div>
      <div class="grid-item item5">
        <button v-on:click="onSubmit" class="submit-btn">Show</button>
        <p>{{ output }}</p>
      </div>
      <div class="converted">
        <div class="exchange-rate">
          <p>Symbol : {{ selectedSymbol }}</p>
          <p v-show="exchangeRate > 0">Rate : {{ exchangeRate }}</p>
          <p v-show="exchangeRate === null">Rate : Not listed</p>
        </div>
        <p class="calculated-amount">
          {{ amount }} {{ symbol1 }} = {{ convertedAmount }} {{ symbol2 }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import utils from "../utils";
import { EventBus, ResponseEventType } from "../eventbus";
import { axiosAPI } from "../axios-lib";

export default {
  data() {
    return {
      amount: null,
      symbol1: "",
      symbol2: "",
      cryptoArray: [],
      fetchedData: [],
      output: null,
      selectedSymbol: "Selected Symbols",
      exchangeRate: null,
      convertedAmount: null,
      //filtered:[],
    };
  },
  created() {
    this.setCryptoArr();
  },
  mounted: function () {
    EventBus.$on(ResponseEventType.FIAT_TO_CRYPTO, (response) => {
      this.fetchedData = response.data;
      this.exchangeRateCalc();
    });
  },
  beforeDestroy() {
    EventBus.$off(ResponseEventType.FIAT_TO_CRYPTO);
  },
  methods: {
    setCryptoArr: function () {
      this.cryptoArray = utils.methods.getCryptoCurrencies();
    },
    onSubmit: function () {
      axiosAPI.fiatToCrypto();
    },
    exchangeRateCalc: function () {
      this.symbol1 = this.symbol1.toUpperCase();
      this.symbol2 = this.symbol2.toUpperCase();
      this.selectedSymbol = this.symbol1.concat(this.symbol2);
      let filtered = [];
      let arr = this.fetchedData;
      filtered = arr.filter((item) => item.symbol === this.selectedSymbol);
      if (filtered.length) {
        this.exchangeRate = filtered[0].price;
      }
      this.convertedAmountCalc();
    },
    convertedAmountCalc: function () {
      this.convertedAmount = this.exchangeRate * this.amount;
    },
  },
};
</script>

<style>
@import "../css/general.css";
@import "../css/crypto.css";

h1 {
  text-align: left !important;
  margin-bottom: 20px;
}
.grid-container {
  display: grid;
}
.grid-item {
  padding: 10px;
  font-size: 20px;
}
.item1,
.item2,
.item3,
.item4,
.item5 {
  grid-column: 1 / span 1;
}
.item3 {
  margin: 20px 0px;
  font-size: 20px !important;
  color: aquamarine;
}
</style>
