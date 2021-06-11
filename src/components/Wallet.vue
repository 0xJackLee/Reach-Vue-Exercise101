<template>
    <div id="wallet">
        <img src="../assets/confi.jpeg" id="wallet-icon" @click="connectWallet">
    </div>
    <div v-if="addr">
        <p>{{addr}}</p>
        <p>{{bal}} next CFX</p>
        <div>
            <img src="../assets/logo.png" id="facuet-icon" @click="fundWallet">
            <p>fund Wallet</p>
        </div>
    </div>
</template>

<script>
export default {
    props: ['addr', 'bal'],
    methods: {
        connectWallet() {
            this.$emit('connectWallet')
        },
        fundWallet() {
            this.$emit('fundWallet')
        },

    }
}
</script>

<style scoped>
 /* style scope is the kind of a standard view scoping for styles that
 allows you to designate styles that only apply to the current file
 */

#wallet {
    position: absolute;
    left: 1vh;
    top: 1vh;
    text-align: left;
}

#wallet-icon {
    max-height: 30vh;
    width: auto;
}

#faucet-icon {
    max-height: 10vh;
    width: auto;
}


</style>