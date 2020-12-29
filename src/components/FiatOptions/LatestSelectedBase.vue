<template>
  <div>
    <div>
      <h3>
        Rates are quoted against the Euro by default. Quote against a different
        currency by setting the base parameter in your request.
      </h3>
      <label>Base = </label>
      <select v-model="selectedBase" class="unit">
        <option value="" disabled selected>Select your option</option>
        <option v-for="fiat in fiatArray">{{ fiat }}</option>
      </select>
      <button v-on:click="renderData" class="show">Show</button>
      <p class="base">Date = {{ date }}</p>
      <table id="data">
        <tr v-for="unit in Object.keys(unitsArr)">
          <td>{{ unit }} : {{ unitsArr[unit].toFixed(3) }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import utils from "../../utils";
import { EventBus, ResponseEventType } from "../../eventbus";
import { axiosAPI } from "../../axios-lib";

export default {
  data() {
    return {
      date: new Date().toLocaleDateString("en-CA"),
      unitsArr: [],
      fiatArray: [],
      selectedBase: "",
    };
  },
  created() {
    this.renderData();
    this.setFiatArr();
  },
  mounted: function () {
    EventBus.$on(ResponseEventType.LATEST_SELECTED_BASE, (response) => {
      this.unitsArr = response.data.rates;
      this.date = response.data.date;
      console.log("this.unitsArr", this.unitsArr);
    });
  },
  beforeDestroy() {
    EventBus.$off(ResponseEventType.LATEST_SELECTED_BASE);
  },
  methods: {
    setFiatArr: function () {
      this.fiatArray = utils.methods.getFiatCurrencies();
    },
    renderData: function () {
      axiosAPI.latestSelectedBaseRequest(this.selectedBase);
    },
  },
};
</script>

<style>
#data {
  display: flex;
  flex-wrap: wrap;
  list-style-type: none;
  padding: 5px;
  margin: 5px;
}
#data tr {
  flex-grow: 1;
  flex-basis: 120px;
  border: 2px solid #cc5a0e;
  margin: 10px;
  background-color: rgb(54, 157, 197);
  color: darkblue;
  font-weight: bold;
  font-size: 13px;
}
.base {
  margin-top: 10px;
  font-size: 20px;
  color: yellow;
}
.unit {
  background-color: white;
  margin: 10px;
  width: 150px;
  font-size: 15px;
  padding: 5px;
}
.show {
  height: 2.5rem;
  font-size: 17px;
  background-color: #82b440;
  font-weight: bold;
  color: white;
  border-radius: 20px;
  padding: 0px 20px;
  text-align: center;
}
h3 {
  margin-bottom: 20px;
}
</style>
