<template>
  <div id="fiatStyle">
    <button class="btn-home">
      <router-link to="/">
        <span class="btn-style">Home Page</span>
      </router-link>
    </button>
    <h1>Fiat - Crypto Currency</h1>
    <div class="grid-container">
    <div class="grid-item item-1">
      <label>Amount = </label>
      <input type="number" v-model="amount" placeholder="Amount" class="input-width" required />
    </div>
    <div class="grid-item item-2">
      <label>From Fiat  : </label>
      <select v-model="fiatUnitFrom" class="input-width">
            <option v-for="fiat in fiatArray">{{fiat}}</option>
      </select>
      <label>To Crypto  : </label>
      <select v-model="cryptoUnitTo" class="input-width">
            <option v-for="crypto in cryptoArray">{{crypto}}</option>
      </select>
    </div>
    <div class="grid-item item-3 info">
      <p>Could not find your pair symbol in dropdown? Type your pair symbol below ...</p>
    </div>
    <div class="grid-item item-4">
      <label>From Fiat  : </label>
      <input type="string" v-model="fiatUnitFrom"  class="input-width"  />
      <label>To Crypto  : </label>
      <input type="string" v-model="cryptoUnitTo"  class="input-width"  />
    </div>
    <div class="grid-item item-5">
      <button v-on:click="onSubmit" class="submit-btn">Show</button>
    </div>
    <div class="converted">
      <div class="exchange-rate">
      <p >Symbol : {{ selectedSymbol }}</p>
      <p  v-show="exchangeRate>0">Rate : {{ exchangeRate }}</p>
      <p  v-show="exchangeRate===null">Rate : Not listed</p>
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
      fiatUnitFrom: "",
      cryptoUnitTo: "",
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
      this.fiatUnitFrom = this.fiatUnitFrom.toUpperCase();
      this.cryptoUnitTo = this.cryptoUnitTo.toUpperCase();
      this.selectedSymbol = this.fiatUnitFrom.concat(this.cryptoUnitTo);
      let filtered = [];
      let arr = this.fetchedData;
      filtered = arr.filter((item) => item.symbol === this.selectedSymbol);
      console.log("filtered", filtered);
      if (filtered.length) {
        this.exchangeRate = filtered[0].price;
      }
      console.log("this.exchangeRate", this.exchangeRate);
      this.convertedAmountCalc();
    },
    convertedAmountCalc: function () {
      this.convertedAmount = this.exchangeRate * this.amount;
    },
  },
};
</script>

<style>
.converted {
  font-size: 25px;
  text-align: center;
  margin: 20px;
  padding: 20px;
  border: 2px solid;
}
#fiatStyle {
  color: white;
  padding: 40px 20px;
}
.btn-home {
  float: right;
  background-color: #82b440;
  border-radius: 35px;
}
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
.input-width {
  margin-left: 10px;
  width: 120px;
  height: 25px;
  background-color: white;
  padding-left: 10px;
}
.btn-style {
  font-size: 20px;
  text-decoration: none;
  padding: 15px;
  color: white;
}
.submit-btn {
  height: 2.5rem;
  font-size: 17px;
  background-color: #82b440;
  font-weight: bold;
  color: white;
  border-radius: 20px;
  padding: 0px 20px;
  text-align: center;
}
.exchange-rate {
  background-color: teal;
  padding: 20px;
}
.calculated-amount {
  font-size: 35px;
  color: yellow;
  margin-top: 20px;
}
</style>