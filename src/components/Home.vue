<template>
  <div>
    <h1>{{ title }}</h1>
    <div class="home">
      <label class="headings">Fiat - Fiat</label>
        <div class="container1 height-block">
          <div class="fiat-style">
            <p class="font-style">Fiat-Fiat CurrencyExchange Rate </p>
            <p class="font-style">You can find many features inside. Try converter button below to find more.. </p>
            <button class="enter-button"> 
              <router-link to="/fiattofiat">
                Converter
              </router-link>
            </button>
          </div >
          <div class="sub-headings"> 
            <span>Latest</span>
            <div class="style-div"></div>
          </div>
        </div>
    </div>
      
    <div class="home">
      <label class="headings">Crypto - Crypto</label>
        <div class="container2">
          <div> 
             <label class="sub-headings"> Popular Cryptocurrency Conversions</label>
            <section>
            <ul class="style-div container4 height-ul">
              <li v-for="unit in Object.keys(popular)">{{popular[unit].symbol}} : {{popular[unit].price}}</li>
            </ul>
            </section>
          </div>
          <div >
            <button class="enter-button"> 
              <router-link to="/cryptotocrypto">
                Converter
              </router-link>
            </button>
          </div >
        </div>
    </div>
    <div class="home">
      <label class="headings">Fiat - Crypto (Vice-Versa)</label>
        <div class="container1 container3">
          <div class="fiat-style"> 
            <p class="sub-headings">Fiat to Crypto </p>
            <button class="enter-button"> 
              <router-link to="/fiattocrypto">
                Converter
              </router-link>
            </button>
          </div>
          <div class="fiat-style">
            <p class="sub-headings">Crypto to Fiat </p>
            <button class="enter-button"> 
              <router-link to="/cryptotofiat">
                Converter
              </router-link>
            </button>
          </div >
        </div>
    </div>  
  </div>
</template>

<script>

import {EventBus,ResponseEventType} from "../eventbus";
import {axiosAPI} from "../axios-lib";

export default {
  components: {},
  data() {
    return {
      title: "Exchange Rate Converter",
      popular:[]
    };
  },
  created() {
    axiosAPI.fiatToCrypto();
  },
  mounted: function(){
     EventBus.$on(ResponseEventType.FIAT_TO_CRYPTO, (response) => {
      this.fetchedData = response.data;this.test();
    });
  },
  methods: {
    test: function() {
      
      let populatUnit= ['BTCUSD','BTCAUD','BTCBRL','BTCCNY','ETHUSD','ETHAUD','ETHBRL','ETHCNY','XRPUSD','XRPAUD','XRPBRL','XRPCNY','BCHUSD','BCHAUD','BCHBRL','BCHCNY']
      
      let dataFetched = this.fetchedData;
      let arr= [];

      let cur = populatUnit.map((s) => {
        let fil = dataFetched.filter((c)=>c.symbol.includes(s));
        arr=arr.concat(fil);
        this.popular= arr;  
      });
    }
  },
};
</script>

<style>
h1 {
  text-align: center;
  padding-top: 20px;
  color: white;
}
.home {
  list-style-type: none;
  margin: 40px 40px 40px 0px !important;
}
.container1,
container3 {
  display: grid;
  grid-template-columns: 50% 50%;
  text-align: center;
  padding: 10px;
  margin: 20px 10px;
  background-color: rgb(4, 24, 75);
  box-shadow: 0 0 20px rgba(14, 6, 6, 0.1);
  border-radius: 5px;
}
.container2 {
  display: grid;
  grid-template-columns: 100%;
  grid-template-rows: 80% 20%;
  text-align: center;
  padding: 10px;
  margin: 20px 10px;
  background-color: rgb(4, 24, 75);
  box-shadow: 0 0 20px rgba(14, 6, 6, 0.1);
  border-radius: 5px;
}
.height-block{
  height:220px;
}
.container3 {
  margin-top: 20px;
  grid-gap: 10px;
}
.container4{
  display: grid;
  grid-template-columns: auto auto auto;
}

.fiat-style {
  background-color: rgb(7, 104, 231) !important;
  color: white !important;
  margin-bottom: 20px;
}
.enter-button {
  width: 100px;
  background-color: #82b440;
  font-size: 14px;
  padding: 0.5rem;
  font-weight: bold;
  text-align: center;
  color: white;
  border-radius: 20px;
  margin-top:20px;
}
.headings {
  color: white;
  font-size: 30px;
  margin: 50px 10px;
}
.sub-headings {
  color: white;
  font-size: 20px;
  margin:10px;
}
.style-div{
  background-color: black;
  margin: 10px;
}
.height-ul{
  height:270px !important;
}

.font-style{
  margin:10px;
  font-size:18px;
  text-align: left;
  font-family:"Helvetica Neue", Helvetica, Arial, sans-serif;
}
</style>
