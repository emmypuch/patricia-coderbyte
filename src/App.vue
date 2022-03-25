<template>
  <div id="app">
    <div class="form">
      <label for="address">Ethereum Address</label>
      <input
        v-model="recipientAddress"
        type="text"
        id="address"
        name="address"
        aria-label="Address to send ethereum"
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
  new Web3.providers.HttpProvider(process.env.VUE_APP_ROSPEN_URL)
);

// Creating a signing account from a private key
const { address: signer } = web3.eth.accounts.wallet.add(
  process.env.VUE_APP_PRIVATE_KEY
);

// eslint-disable-next-line
const contract = new web3.eth.Contract(CONTRACT_ABI, CONTRACT_ADDRESS);
console.log(contract, signer);

export default {
  name: "App",
  data() {
    return {
      recipientAddress: "",
      amount: "0",
      loading: false,
    };
  },
  methods: {
    async transfer() {
      this.loading = true;
      const tx = contract.methods.transfer(
        this.recipientAddress,
        web3.utils.toWei(this.amount.toString())
      );

      const nonce = await web3.eth.getTransactionCount(signer);

      const gasPrice = await web3.eth.getGasPrice();

      const gasLimit = await web3.eth.getBlock("latest");

      const data = tx.encodeABI();
      const txData = {
        from: signer,
        to: contract.options.address,
        data,
        gasPrice,
        gasLimit: gasLimit.gasLimit,
        nonce,
      };

      try {
        const receipt = await web3.eth.sendTransaction(txData);
        console.log(receipt);
        this.loading = false;
      } catch (err) {
        console.log(err);
        this.loading = false;
      }
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
