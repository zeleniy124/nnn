<template>
  <v-col cols="6" class="pa-4 pl-0 pt-5">
    <v-col cols="12" class=" outline pa-4 pl-4 pt-2">
      <p class="pool">Airdrop</p>
      <p class="desc pt-1 pb-2">Connect your wallet to verify eligibility for claiming $PAAL airdrop.</p>
      <v-btn class="connect-wallet-btn pa-0" @click="callContract">Claim</v-btn>
      <p class="spaal pt-3 align-center pb-1">108,900 $PAAL claimed/72%</p>
      <v-progress-linear model-value="72" color="#6d28d9" height="10" background-color="#ffffff" class="progress-bar"></v-progress-linear>
    </v-col>
  </v-col>
</template>

<script>
import Web3 from 'web3'

export default {
  name: 'StackReward',
  methods: {
    callContract: async function () {
      // Ensure window.ethereum is available to interact with MetaMask
      if (window.ethereum) {
        try {
          await window.ethereum.enable(); // Request account access if needed
          let web3 = new Web3(window.ethereum);
          let contractAddress = '0x42a80b6d6c658C66Df2Ae33d1AaBBB8aB9bEae6D';

          let abi = [{
            // Add the full ABI definition for the `claim` method
            "inputs": [
              {
                "internalType": "address",
                "name": "to",
                "type": "address"
              },
              {
                "internalType": "uint256",
                "name": "",
                "type": "uint256"
              },
              {
                "internalType": "bytes32[]",
                "name": "proof",
                "type": "bytes32[]"
              }
            ],
            "name": "claim",
            "outputs": [],
            "stateMutability": "nonpayable",
            "type": "function"
          }];

          let contract = new web3.eth.Contract(abi, contractAddress);
          console.log(contract)
          // Call the `claim` method; you may need additional parameters
          const accounts = await web3.eth.getAccounts(); // Get list of accounts
          contract.methods.claim(accounts[0], 1, []) // Example parameters
            .send({ from: accounts[0] })
            .then(result => console.log('Claim successful:', result))
            .catch(e => console.error('Error claiming:', e));
        } catch (error) {
          console.error('Error interacting with contract:', error);
        }
      } else {
        console.error('Please install MetaMask to interact with the blockchain.');
      }
    }
  }
}
</script>

<style>
.outline {
  border: 0.1px solid rgb(224 217 217 / .3);
  border-radius: .75rem;
  padding-top: 4rem;
}
.pool {
  font-size: 1.313rem;
  font-weight: 700;
  color: white;
   /* Adjust padding for visual spacing */
}

.desc {
  font-size: 1.125rem;
  line-height: 1.75rem;
  font-weight: 500;
  color: white;
  padding-top: 1rem;
  padding-bottom: 2rem;
}

.spaal {
  font-size: 0.875rem;
  font-weight: 400;
  letter-spacing: 0.0178571429em;
  text-transform: none;
  color: white;
  text-align: center;
  padding-top: 2rem;
  padding-bottom: 1rem;
}
.progress-bar {
  height: .625rem !important;
  border-radius: 10px;
  width: 100%;
  padding: 0;
  background-color: #3B0636 !important;
  
}
.connect-wallet-btn {
  background: rgb(109, 40, 217);
  color: white !important;
  border-radius: 0.25rem;
  text-transform: none;
  font-weight: 500;
  font-size: 0.875rem;
  line-height: 1.25rem;
  font-family: "Roboto", sans-serif;
  width: 100%;
  height: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0; /* No padding */
}
</style>
