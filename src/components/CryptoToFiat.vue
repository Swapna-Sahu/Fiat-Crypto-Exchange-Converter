<template>
  <div id="fiatStyle">
    <button class="btn-home">
      <router-link to="/">
        <span class="btn-style">Home Page</span>
      </router-link>
    </button>
    <h1>Crypto - Fiat Currency</h1>
    <div class="grid-container">
      <div class="grid-item item-1">
        <label>Amount = </label>
        <input
          type="number"
          v-model="amount"
          placeholder="Amount"
          class="input-width"
          required
        />
      </div>
      <div class="grid-item item-2">
        <label>From Crypto: </label>
        <select v-model="cryptoUnitFrom" class="input-width">
          <option v-for="crypto in cryptoArray">{{ crypto }}</option>
        </select>
        <label>To Fiat: </label>
        <select v-model="fiatUnitTo" class="input-width">
          <option v-for="fiat in fiatArray">{{ fiat }}</option>
        </select>
      </div>
      <div class="grid-item item-3 info">
        <p>
          Could not find your pair symbol in dropdown? Type your pair symbol
          below ...
        </p>
      </div>
      <div class="grid-item item-4">
        <label>From Crypto : </label>
        <input type="string" v-model="cryptoUnitFrom" class="input-width" />
        <label>To Fiat : </label>
        <input type="string" v-model="fiatUnitTo" class="input-width" />
      </div>
      <div class="grid-item item-5">
        <button v-on:click="onSubmit" class="submit-btn">Show</button>
      </div>
      <div class="converted">
        <div class="exchange-rate">
          <p>Symbol : {{ selectedSymbol }}</p>
          <p v-show="exchangeRate > 0">Rate : {{ exchangeRate }}</p>
          <p v-show="exchangeRate === null">Rate : Not listed</p>
        </div>
        <p class="calculated-amount">Output : {{ convertedAmount }}</p>
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
      cryptoUnitFrom: "",
      fiatUnitTo: "",
      cryptoArray: [],
      fiatArray: [],
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
    this.setFiatArr();
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
    setFiatArr: function () {
      this.fiatArray = utils.methods.getFiatForCryptoCurrencies();
    },
    setCryptoArr: function () {
      this.cryptoArray = utils.methods.getCryptoCurrencies();
    },
    onSubmit: function () {
      axiosAPI.fiatToCrypto();
    },
    exchangeRateCalc: function () {
      this.fiatUnitTo = this.fiatUnitTo.toUpperCase();
      this.cryptoUnitFrom = this.cryptoUnitFrom.toUpperCase();
      this.selectedSymbol = this.cryptoUnitFrom.concat(this.fiatUnitTo);
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
.item-1,
.item-2,
.item-3,
.item-4,
.item-5 {
  grid-column: 1 / span 1;
}
.info {
  margin: 20px 0px;
  font-size: 20px !important;
  color: aquamarine;
}
</style>