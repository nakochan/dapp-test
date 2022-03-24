<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="sendToken">
      보내기
    </button>
  </div>
</template>

<script>
import web3 from "web3";

const {ethereum} = window;

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  async mounted() {
    if (typeof ethereum !== 'undefined') {
      console.log('MetaMask is installed!');
    }
    const accounts = await ethereum.request({method: 'eth_requestAccounts'});
    const account = accounts[0];

    ethereum.on('accountsChanged', function (accounts2) {
      // Time to reload your interface with accounts[0]!
      console.log(accounts2);
    });

    console.log(account);
// We currently only ever provide a single account,
// but the array gives us some room to grow.
  },
  methods: {
    async sendToken() {
      const transactionParameters = {
        // value: '0x29a2241af62c0000',
        value: web3.utils.toWei(String('0.008'), 'ether'),
        // gasPrice: '0x09184e72a000',
        to: '0x0000000000000000000000000000000000000000',
        from: ethereum?.selectedAddress, // must match user's active address.
        data:
            '0x7f7465737432000000000000000000000000000000000000000000000000000000600057', // Optional, but used for defining smart contract creation and interaction.
        chainId: '0x3', // Used to prevent transaction reuse across blockchains. Auto-filled by MetaMask.
      };

// txHash is a hex string
// As with any RPC call, it may throw an error
      const txHash = await ethereum.request({
        method: 'eth_sendTransaction',
        params: [transactionParameters],
      });

      console.log(txHash);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
