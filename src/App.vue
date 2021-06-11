<template>
  <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </div>
  <router-view />
  <wallet :addr="addr" :bal="bal" @connectWallet="connectWallet" @fundWallet="fundWallet"></wallet>
</template>
<script>
import * as reach from "@reach-sh/stdlib/ETH.mjs";
import Wallet from "./components/Wallet.vue";

export default {
  name: "App",

  data() {
    return {
      // interface that reach provide to call all of our wallet functions
      acc: undefined,
      addr: undefined, // address
      bal: undefined, // balance
      balAtomic: undefined, // atomic balance which reach provides by default
    };
  },
  // methods: {
  //   async updateBalance() {
  //     try {
  //       this.balAtomic = await reach.balanceOf(this.acc)
  //       this.bal = String(reach.formatCurrency(this.balAtomic)).substr(0,6)
  //     } catch (err) {
  //       console.log(err)
  //     }

  //   },
  //   async connectWallet() {
  //     try {
  //       this.acc = await reach.getDefaultAccount()
  //       this.addr = await this.acc.getAddress()
  //       this.bal = await reach.balanceOf(this.acc)
  //     }
  //     catch (err) {
  //       console.log(err)
  //     }
  //   },
  //   async fundWallet() {
  //     this.faucetLoading = true
  //     try {
  //       const fundAmt = 10
  //       await reach.fundFromFaucet(this.acc, reach.parseCurrency(fundAmt))
  //       this.updateBalance()
  //     } catch (err) {
  //       console.log(err)
  //     }
  //     this.faucetLoading = false
  //   }
  // },

  methods: {
    // using async because we'll relying on reach functions that are likewise asynchronous
    // as they have to get the wallet state from either the develop network itself or from wallet manager
    // browser extensions, which it doesn't have direct access to without user content

    // Connect wallet function
    async connectWallet() {
      try {
        this.acc = await reach.getDefaultAccount(); // get account
        this.addr = await this.acc.getAddress(); // get address
        this.updateBalance();
      } catch (error) {
        console.log(error);
      }
    },
    // Update balance function
    async updateBalance() {
      try {
        this.balAtomic = await reach.balanceOf(this.acc); // get atomic balance
        this.bal = await reach.formatCurrency(this.balAtomic);
      } catch (error) {
        console.log(error);
      }
    },

    // Fund the wallet function
    async fundWallet() {
      try {
        await reach.fundFromFaucet(this.accc, reach.parseCurrency(10)); // reach.parseCurrency(10) parse 10 eth into the atomic eth uint to fund the account
      } catch (error) {
        console.log(error);
      }
    },
  },
  components: {
    Wallet,
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
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
