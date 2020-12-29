<template>
  <div>
    <nav>
      <img
        src="./assets/bitcoin-btc-logo.png"
        alt="company logo"
        class="logo"
      />
      <span>Exchange rate</span>
      <button class="nav-btn">Login</button>
      <button class="nav-btn">Sign up</button>
    </nav>

    <div class="container">
      <ul>
        <li>Home</li>
        <li>Features</li>
        <li>Services</li>
        <li>Contact us</li>
      </ul>
      <Header>
        <div class="top-left">Digital Currencies at Your Fingertips</div>
        <p>
          We want to open the market to everyone, with an easy-to-use, intuitive
          interface. To help clients better manage their finances, we developed
          innovative trading features:
        </p>
      </Header>
      <div class="exchange">
        <router-view></router-view>
      </div>
      <Footer>
        <h2>Sign Up For Newsletters</h2>
        <input type="email" placeholder="Your email address" required />
        <button>SUBSCRIBE</button>
        <div>
          <p>Swapna Sahu © 2020. All rights reserved.</p>
        </div>
      </Footer>
    </div>
  </div>
</template>

<script>
import Home from "./components/Home";
import FiatToFait from "./components/FiatToFiat";
import CryptoToCrypto from "./components/CryptoToCrypto";
import FiatToCrypto from "./components/FiatToCrypto";
import CryptoToFiat from "./components/CryptoToFiat";

export default {
  components: {
    home: Home,
    "fiat-fiat": FiatToFait,
    "crypto-crypto": CryptoToCrypto,
    "fiat-crypto": FiatToCrypto,
    "crypt-tofiat": CryptoToFiat,
  },

  data() {
    return {
      title: "Exchange Rate",
    };
  },
};
</script>

<style >
html {
  min-height: 100%;
}

html body {
  min-height: 100%;
  background-color: rgb(67, 67, 253);
}
body {
  min-height: 100vh;
  background-image: url("./assets/fiat-crypto.jpg");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  background-blend-mode: multiply;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}
nav {
  width: 100%;
  height: 50px;
  font-size: 12px;
  background-color: #262626;
  margin-top: 20px;
}
span {
  color: rgb(111, 250, 118);
  font-size: 35px;
}
.container {
  background-color: #332d2d;
  margin: 20px;
}

ul {
  height: 55px;
  background-color: rgb(4, 4, 66);
  margin: 5px;
  display: flex;
  list-style-type: none;
}
li {
  width: 198px;
  background-color: #82b440;
  font-size: 14px;
  color: white;
  margin: 10px 20px;
  padding-top: 0.5rem;
  text-align: center;
}

header {
  height: 400px;
  background-image: url("./assets/home.jpg");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  background-clip: border-box;
  -webkit-background-clip: border-box;
  position: relative;
}
header p {
  position: absolute;
  top: 120px;
  left: 20px;
  color: white;
  font-weight: 400;
  font-size: 20px;
  width: 70%;
  margin-top: 8rem;
}

.btn {
  float: right;
  width: 100px;
  height: 40px;
}
.exchange {
  margin: 1rem;
  background-blend-mode: soft-light;
  background-color: rgb(24, 13, 71);
}
.top-left {
  position: absolute;
  font-size: 50px;
  top: 5rem;
  left: 20px;
  color: white;
  font-weight: bold;
  width: 50%;
  margin-bottom: 5rem;
}
.nav-btn {
  float: right;
  background-color: #82b440;
  border-radius: 35px;
  font-size: 14px;
  padding: 10px 25px;
  color: white;
  margin: 5px 10px;
}
.container section nav {
  background-color: white;
}

footer {
  color: white;
  height: 32vh;
  background-color: rgb(43, 39, 73);
  padding: 20px;
  text-align: center;
}

footer h2 {
  margin: 10px;
  text-decoration-line: underline;
}
footer input {
  background-color: white;
  margin: 20px;
  padding: 10px;
  width: 300px;
}

footer button {
  color: white;
  background-color: blue;
  width: 200px;
  height: 39px;
  padding: 0px 10px;
}

footer div p {
  margin: 20px;
}
.logo {
  width: 30px;
  height: 30px;
}
</style>
