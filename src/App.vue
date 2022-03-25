<template>
  <div id="app">
    <div class="form">
      <label for="address">Ethereum Address</label>
      <input
        v-model="recipientAddress"
        type="text"
        id="address"
        name="address"
        aria-label="Address to send eth"
      />
      <label for="amount">Enter Amount</label>
      <input type="number" id="amount" name="amount" v-model="amount" />
      <div>
        <button @click="transfer" :disabled="loading">Make Transfer</button>
      </div>
    </div>
  </div>
</template>

<script>
// eslint-disable-next-line
import { CONTRACT_ABI, CONTRACT_ADDRESS } from "./data/contract-info";
const Web3 = require("web3");
const web3 = new Web3(
  new Web3.providers.HttpProvider(
    "https://ropsten.infura.io/v3/45fb8c5b270b4e02bbb9b49f37b19ba9"
  )
);
// Creating a signing account from a private key
const signer = web3.eth.accounts.privateKeyToAccount(
  "cb4917f896c502fb844b59555e2f586464659b87c307e5ab563099dfccbba66e"
);
web3.eth.accounts.wallet.add(signer);
// web3.eth.defaultAccount = CONTRACT_ADDRESS;
const contract = new web3.eth.Contract(CONTRACT_ABI);

export default {
  name: "App",
  data() {
    return {
      recipientAddress: "",
      amount: 0,
      loading: false,
    };
  },
  async mounted() {
    const accounts = await web3.eth.getAccounts();
    console.log(accounts);
    // console.log(web3.eth);
  },
  methods: {
    transfer() {
      this.loading = true;
      web3.eth
        .sendTransaction({
          from: contract.defaultAccount,
          to: this.recipientAddress,
          value: this.amount,
        })
        .then((receipt) => {
          console.log(receipt);
          this.recipientAddress = "";
          this.amount = 0;
        })
        .catch((error) => {
          console.error(error);
        });
      this.loading = false;
    },
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

.form {
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
  background: rgb(179, 219, 219);
}
</style>
