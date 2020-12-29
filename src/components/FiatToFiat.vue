<template>
  <div id="fiatStyle">
    <button  class="btn-home">
      <router-link to="/">
        <span class="btn-style">Home Page</span>
      </router-link>
    </button>
    <h1>Fiat - Fiat Exchange Rate</h1>
    <div>
      <div id="output">
        <component v-bind:is="component"></component>
      </div>
      <div id="options">
        <button v-on:click="component = 'exchange-rate'" class="btn-options">
          Exchange Rate
        </button>
        <button v-on:click="component = 'latest-base-eur'" class="btn-options">
          Latest in base EUR
        </button>
        <button v-on:click="component = 'history-base-eur'" class="btn-options">
          History in base EUR
        </button>
        <button v-on:click="component = 'latest-selected-base'" class="btn-options">
          Latest in selected base
        </button>
        <button v-on:click="component = 'two-symbol-base-eur'" class="btn-options">
           Two symbol base EUR
        </button>
        <button v-on:click="component = 'history-time-period'" class="btn-options">
          History time period
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import FiatExchangeRate from "./FiatOptions/FiatExchangeRate";
import HistoryBaseEur from "./FiatOptions/HistoryBaseEur";
import HistoryTimePeriod from "./FiatOptions/HistoryTimePeriod";
import LatestBaseEur from "./FiatOptions/LatestBaseEur";
import TwoSymbolBaseEur from "./FiatOptions/TwoSymbolBaseEur";
import LatestSelectedBase from "./FiatOptions/LatestSelectedBase";

export default {
  components: {
    "exchange-rate": FiatExchangeRate,
    "history-base-eur": HistoryBaseEur,
    "history-time-period": HistoryTimePeriod,
    "latest-base-eur": LatestBaseEur,
    "two-symbol-base-eur": TwoSymbolBaseEur,
    "latest-selected-base": LatestSelectedBase,
  },
  data() {
    return {
      component: "exchange-rate",
    };
  },
  methods: {
  },
};
</script>

<style scoped>
#fiatStyle{
color:white;
padding:40px 20px ;
}
h1{
  margin-bottom: 40px;
}
.btn-home{
  float: right;
    background-color: #82b440;
    border-radius: 35px;
}
#output {
  display: inline-grid;
  width: 60%;
  padding-left: 10px;
}
#options {
  display: inline-grid;
  row-gap: 10px;
  margin-left: 20px;
  width: 30%;
}
.btn-style{
  font-size: 20px;
  text-decoration: none;
  padding:15px;
  color:white;
}
.btn-options{
  background-color: lightgray;
  color:darkblue;
  font-size: 20px;
  font-weight: bold;
  height:30px;
  padding:0px 5px;
}
</style>
