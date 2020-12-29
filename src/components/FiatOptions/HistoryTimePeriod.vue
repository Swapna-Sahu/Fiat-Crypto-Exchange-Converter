<template>
<div>
  <div>
    <div>
      <h3>Get historical rates for a time period.</h3>
      <p class="base">Base = EUR</p>
      <label>Start Date</label>
      <input
        type="date"
        v-model="startDate"
        value=""
        placeholder="yyyy-mm-dd"
        class="time-style"
        required
      />
      <label>End Date</label>
      <input
        type="date"
        v-model="endDate"
        value=""
        placeholder="yyyy-mm-dd"
        class="time-style"
        required
      />
      <button v-on:click="renderData" class="show">Show</button>
                <table id="data">
        <tr v-for="unit in Object.keys(unitsArr)">
          <td class="base"><p>{{ unit }}</p><br>
          <table>
          <tr v-for="item in Object.keys(unitsArr[unit])">
          <td><p>{{item}}: {{ unitsArr[unit][item] }}</p></td>
        </tr>
      </table>
          </td>
        </tr>
      </table>
 
    </div>
    
  </div>


</div> 
</template>

<script>
import { EventBus, ResponseEventType } from "../../eventbus";
import { axiosAPI } from "../../axios-lib";

export default {
  data() {
    return {
      unitsArr: [],
      startDate: "",
      endDate: "",
      selectedUnit: "INR",
    };
  },
  created() {
    this.renderData();
  },
  mounted: function () {
    EventBus.$on(ResponseEventType.HISTORY_TIME_PERIOD, (response) => {
      this.unitsArr = response.data.rates;
      this.selectedUnitReport();
    });
  },
  beforeDestroy() {
    EventBus.$off(ResponseEventType.HISTORY_TIME_PERIOD);
  },
  methods: {
    renderData: function () {
      axiosAPI.historyTimePeriod(this.startDate, this.endDate);
    },
    selectedUnitReport() {
      let b = this.unitsArr;
      let u = this.selectedUnit;
      let first = Object.keys(b);
      console.log("first", b);
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
.base {
  margin-top: 10px;
  font-size: 20px;
  color: yellow;
}
.text-center{
  display: flex;
  flex-wrap: wrap;
  list-style-type: none;
}
.text-center li {
  flex-grow: 1;
  flex-basis: 10px;
  background-color: rgb(54, 157, 197);
  width:0px !important;
  margin:6px !important;
  padding: 0px !important;
}

</style>
