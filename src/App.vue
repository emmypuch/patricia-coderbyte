<template>
  <div id="app">
    <form>
      <label for="address">Ethereum Address</label>
      <input
        type="text"
        id="address"
        name="address"
        placeholder="Ethereum Address"
        aria-label="Address to send eth"
      />

      <label for="amount">Enter Amount</label>
      <input type="number" id="amount" name="amount" />
      <div>
        <button @click="submit" type="submit">Submit</button>
      </div>
    </form>
  </div>
</template>

<script>
const Web3 = require("web3");
const web3 = new Web3(Web3.givenProvider || "http://localhost:8545");
import { CONTRACT_ABI } from "./data/contract-info";
const contract = new web3.eth.Contract(CONTRACT_ABI);

export default {
  name: "App",
  components: {},
  mounted() {
    console.log(contract);
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
}

form {
  background-color: teal;
  padding: 4rem;
  width: 60%;
  height: 250px;
  border-radius: 5px;
}

label {
  display: flex;
  color: rgb(221, 236, 236);
  font-size: 1.1rem;
}

label[for="number"] {
  margin-top: 10px;
}

input[type="text"],
input[type="number"] {
  border: none;
  border-radius: 5px;
  width: 95%;
  outline: none;
  color: rgb(83, 170, 170);
  font-size: 1rem;
  display: inline-block;
  padding: 12px 20px;
  margin: 10px 0;
}

input[type="text"] {
  margin-bottom: 15px;
}

input[type="number"] {
  margin-top: 15px;
}

::placeholder {
  color: rgb(83, 170, 170);
}

button {
  margin-top: 30px;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  text-align: center;
  padding: 10px 30px;
  color: teal;
  cursor: pointer;
}

button:hover {
  background: rgb(70, 158, 158);
  transition: 3s ease-in;
}
</style>
