<template>
  <div>
    <div>
      <h2>Get historical rates for any day since 1999.</h2>
      <p class="base">Base = EUR</p>
      <label>Date =</label>
      <input type="date" v-model="date" required />
      <button v-on:click="renderData" class="show" >Show</button>
      <table id="data" >
        <tr v-for="unit in Object.keys(unitsArr)">
          <td >{{unit}} : {{unitsArr[unit].toFixed(3)}}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import { axiosAPI } from "../../axios-lib";
import { EventBus, ResponseEventType } from "../../eventbus";

export default {
  data() {
    return {
      unitsArr: [],
      date: "",
    };
  },
  created() {
    this.renderData();
  },
  mounted: function () {
    EventBus.$on(ResponseEventType.HISTORY_BASE_EUR, (response) => {
      this.unitsArr = response.data.rates;
      this.date = response.data.date;
      console.log("this.unitsArr", this.unitsArr);
    });
  },
  beforeDestroy() {
    EventBus.$off(ResponseEventType.HISTORY_BASE_EUR);
  },
  methods: {
    renderData: function () {
      axiosAPI.historyBaseEurRequest(this.date);
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

input {
  background-color: white;
  margin: 10px;
}
.show{
  height:2.5rem;
  font-size: 17px;
  background-color: #82b440;
  font-weight: bold;
  color: white;
  border-radius: 20px;
  padding:0px 20px;
  text-align: center;
}
.base{
  margin-top: 10px;
  font-size:20px;
  color:yellow;
}
</style>
