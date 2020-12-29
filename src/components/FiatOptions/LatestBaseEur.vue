<template>
  <div>
    <div>
        <h2>Latest with Base = EUR</h2>
        <p class="base">Date : {{date}}</p>
        <table id="data" >
        <tr v-for="unit in Object.keys(unitsArr)">
          <td >{{unit}} : {{unitsArr[unit]}}</td>
        </tr>
        </table>
    </div>
  </div>  
</template>

<script>
import { axiosAPI } from "../../axios-lib";
import {EventBus,ResponseEventType} from "../../eventbus";

export default {
  data() {
    return {
      unitsArr: [],
      date:'',
    };
  },
  created() {
    this.renderData();
  },
  mounted: function(){
    EventBus.$on(ResponseEventType.LATEST_BASE_EUR, (response) => {
      this.unitsArr = response.data.rates;
      this.date = response.data.date;
      console.log('this.unitsArr',this.unitsArr);
    });
  },
  beforeDestroy() {
    EventBus.$off(ResponseEventType.LATEST_BASE_EUR);
  },
  methods: {
    renderData: function() {
      axiosAPI.latestRequest();
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
    margin:10px;
    background-color: rgb(54, 157, 197);
    color:darkblue;
    font-weight: bold;
    font-size: 14px;
}
.base{
  margin-top: 10px;
  font-size:20px;
  color:yellow;
}
</style>
