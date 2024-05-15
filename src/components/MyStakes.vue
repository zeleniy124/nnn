<template>
  <v-row>
    <v-col cols="12" class="pd">
      <div class="my-stakes ">
        <h2 class="stakes-title">My Stakes & Rewards</h2>
        <v-card class="wallet-card px-6 py-15">
          <v-card-text class="d-flex flex-column align-center justify-center text-center ">
            <img
              :src="empty"
              class="wallet-icon"
            />
            <div class="wallet-connect-text">
              Connect your wallet
            </div>
            <v-btn class="connect-wallet-btn px-0" @click="connect" v-if="alreadyConnectedWallets.length === 0" >Connect wallet</v-btn>
            <v-btn class="connect-wallet-btn px-0" @click="callContract" v-else>Stake now</v-btn>
          </v-card-text>
        </v-card>
      </div>
    </v-col>
  </v-row>
</template>


<script>
import { init, useOnboard } from '@web3-onboard/vue';
import injectedModule from '@web3-onboard/injected-wallets';
import Web3 from 'web3'

export default {
  
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
  },
  setup() {
    const injected = injectedModule();
    const infuraKey = 'c2d91ad4b16242f28d2e840457d8a1ce';
    const rpcUrl = `https://mainnet.infura.io/v3/${infuraKey}`;

    // eslint-disable-next-line no-unused-vars
    const web3Onboard = init({
      wallets: [injected],
      chains: [
        {
          id: '0x1',
          token: 'ETH',
          label: 'Ethereum Mainnet',
          rpcUrl
        },
        {
          id: 42161,
          token: 'ARB-ETH',
          label: 'Arbitrum One',
          rpcUrl: 'https://rpc.ankr.com/arbitrum'
        },
        {
          id: '0xa4ba',
          token: 'ARB',
          label: 'Arbitrum Nova',
          rpcUrl: 'https://nova.arbitrum.io/rpc'
        },
        {
          id: '0x2105',
          token: 'ETH',
          label: 'Base',
          rpcUrl: 'https://mainnet.base.org'
        }
      ]
    });

    const { alreadyConnectedWallets, connectWallet } = useOnboard();
    const connect = async () => {
      console.log("Attempting to connect wallet...");
      try {
        await connectWallet();
      } catch (error) {
        console.error('Error connecting to wallet:', error);
      }
    };
    return { alreadyConnectedWallets, connect };
},
  data() {
      return {
        empty: require('@/assets/empty.png'),
      };
    },
}
</script>

<style scoped>
.my-stakes {
  width: 100%; /* Ensure the container takes full width */
}

.stakes-title {
  text-align: left;
  color: white;
  font-size: 1.313rem; /* Adjust the font size as needed */
  line-height: 1.5;
  font-weight: 700;
  margin-bottom: 1.25rem; /* Space between title and card */
}

.wallet-card {
  background-color: rgb(14 2 22 / 1); /* Make card background transparent */
  color: white;
  border: 0.1px solid rgb(224 217 217 / .3);
  border-radius: .75rem;

}

.wallet-icon {
  margin-bottom: 1rem; /* Space between icon and text */
  height: 6rem;
  width: auto;
}
.connect-wallet-btn.non-clickable {
  pointer-events: none;  /* Prevents all click, state and hover interactions */
  opacity: 0.5;  /* Optional: visually indicate the button is inactive */
}
.wallet-connect-text {
  margin-bottom: 1rem; /* Space between text and button */
  font-weight: 500;
  font-size: 1.25rem;
  line-height: 1.75rem;

}

.connect-wallet-btn {
  background: rgb(109, 40, 217);
  color: white;
  border-radius: 0.25rem;
  text-transform: none;
  font-weight: 500;
  font-size: .875rem;
  line-height: 1.25rem;
  font-family: "Roboto", sans-serif;
}

.connect-wallet-btn:hover {
  background-color: #9d50bb; /* Button hover color */
}

/* Use this if you want to remove the Vuetify card's default padding */
.v-card-text {
  padding: 0 !important;
}
.pd {
  padding-top: 2rem;
}
</style>
