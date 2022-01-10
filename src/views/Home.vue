<template>
  <div class="text-center">
    <h2>home</h2>
    <h3>is activated: {{ isActivated }}</h3>
    <div v-if="isActivated">
      <p>total supply: {{ totalSupply }}</p>
      <p>balance of my account is: {{ balance }} BLT</p>
      address: {{ address }}
      <p>token name: {{ tokenName }}</p>
      <p>token symbol: {{ tokenSymbol }}</p>

      <p @click="changeNumber">change number:: {{ balanceResult }}</p>
      <p>
        change balance number to be more readable
        <span @click="changeNumber" class="btn btn-outline-primary">
          Click here {{ balanceResult }}</span
        >
        {{ balanceResult }}
      </p>
    </div>
  </div>
</template>

<script lang="ts">
import { useEthers, displayEther } from "vue-dapp";
import { ethers } from "ethers";
import BlazTokenAbi from "../abi/BlazTokenAbi.json";

export default {
  name: "Home",
  created() {
    if (this.signer) {
      this.contractTotalSupply();
      this.contractBalanceOf();
      this.nameOfToken();
      this.symbol();
    }
  },
  data() {
    totalSupply: null;
    balance: null;
    tokenName: null;
    tokenSymbol: "";
    balanceResult: "";
    // contractAdress: this.address
  },
  computed: {
    // changeNumber() {
    //   let x = 10 ** 18;
    //   let balanceResult = this.balance / x;
    //   console.log("balance result: " + balanceResult);
    // },
  },

  methods: {
    changeNumber() {
      let x = 10 ** 18;
      let balanceResult = this.balance / x;
      console.log("balance result: " + balanceResult);
    },
    // Total amount of tokens
    async contractTotalSupply() {
      this.totalSupply = await this.contract.totalSupply();
      this.totalSupply / 10 ** 18;
      console.log("total supply:" + this.totalSupply);
    },

    // tukaj mormo dobit od accounta number
    async contractBalanceOf() {
      console.log("balance before");
      this.balance = await this.contract.balanceOf(this.address);
      console.log("balance of my account is:" + this.balance);
    },

    // Token name
    async nameOfToken() {
      this.tokenName = await this.contract.name();
      console.log("name" + this.tokenName);
    },

    // Token symbol
    async symbol() {
      this.tokenSymbol = await this.contract.symbol();
      console.log("token symbol: " + this.tokenSymbol);
    },
  },

  setup() {
    const { address, balance, chainId, isActivated, signer } = useEthers();
    const contractInterface = new ethers.utils.Interface(BlazTokenAbi);
    const contractAddress = "0x47a2f6B35ecF95d888a0D55c87Cc0FA313707385";
    const contract = new ethers.Contract(
      contractAddress,
      contractInterface,
      signer.value
    );
    return {
      address,
      balance,
      chainId,
      isActivated,
      displayEther,
      signer,
      contract,
    };
  },
};
</script>
