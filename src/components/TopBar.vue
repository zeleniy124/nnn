<template>
  <header class="custom-top-bar">
    <v-container fluid class="pa-0">
      <v-row align="center" justify="space-between" class="py-0 px-4">
        <p class="pool pl-4">Rewards & Staking</p>
        <!-- Check if there are already connected wallets -->
        <div v-if="alreadyConnectedWallets.length > 0" class="wallet-info d-flex align-items-center">
          <!-- Dynamically display wallet icon and formatted address -->
          <img v-if="formattedWalletAddress" :src="walletIcon" class="wallet-icon"/>
          <span class="text_main wallet-address">{{ formattedWalletAddress }}</span>
          <v-btn class="disconnect_btn ml-auto" @click="disconnect">
            Disconnect wallet
          </v-btn>
        </div>
        <v-btn v-else class="con_wal" @click="connect">
          Connect Wallet
        </v-btn>
      </v-row>
    </v-container>
  </header>
</template>


<script>
import { ref, computed, onMounted } from 'vue';
import { init, useOnboard } from '@web3-onboard/vue';
import injectedModule from '@web3-onboard/injected-wallets';
import walletConnectModule from '@web3-onboard/walletconnect';

import Web3 from 'web3';




export default {
  name: 'TopBar',
  setup() {
    const injected = injectedModule();
    const infuraKey = 'c2d91ad4b16242f28d2e840457d8a1ce';
    const rpcUrl = `https://mainnet.infura.io/v3/${infuraKey}`;
    const wcInitOptions = {
    /**
     * Project ID associated with [WalletConnect account](https://cloud.walletconnect.com)
     */
    projectId: '7c59514fc65fed5787f730c10babae37',
    /**
     * Chains required to be supported by all wallets connecting to your DApp
     */
    requiredChains: [1],
    /**
     * Chains required to be supported by all wallets connecting to your DApp
     */
    optionalChains: [],
    /**
     * Defaults to `appMetadata.explore` that is supplied to the web3-onboard init
     * Strongly recommended to provide atleast one URL as it is required by some wallets (i.e. MetaMask)
     * To connect with WalletConnect
     */
    dappUrl: 'http://YourAwesomeDapp.com'
  }

  // initialize the module with options
  const walletConnect = walletConnectModule(wcInitOptions)
    // eslint-disable-next-line no-unused-vars
    const web3Onboard = init({
      wallets: [injected, walletConnect],
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

    const { connectWallet, disconnectWallet, alreadyConnectedWallets } = useOnboard();
    const walletAddress = ref('');
    const walletIcon = require('@/assets/wallet.svg'); // Load the wallet icon dynamically

    async function fetchWalletAddress() {
      if (window.ethereum) {
        const web3 = new Web3(window.ethereum);
        try {
          const accounts = await web3.eth.getAccounts();
          if (accounts.length > 0) {
            walletAddress.value = accounts[0];
          } else {
            walletAddress.value = '';
          }
        } catch (error) {
          console.error("Error fetching accounts:", error);
          walletAddress.value = '';
        }
      }
    }

    // Computed property for formatting the wallet address
    const formattedWalletAddress = computed(() => {
      if (walletAddress.value.length > 0) {
        return `${walletAddress.value.substring(0, 5)}...${walletAddress.value.substring(walletAddress.value.length - 4)}`;
      }
      return '';
    });

    // Fetch the wallet address on component mount
    onMounted(fetchWalletAddress);

    const disconnect = async () => {
      try {
        await disconnectWallet({ label: 'MetaMask' });
        console.log("Wallet disconnected");
        walletAddress.value = '';  // Clear the wallet address on disconnect
      } catch (error) {
        console.error('Error disconnecting wallet:', error);
      }
    };

    const connect = async () => {
      console.log("Attempting to connect wallet...");
      try {
        await connectWallet();
        
      } catch (error) {
        console.error('Error connecting to wallet:', error);
      }
    };

    return { connect, disconnect, alreadyConnectedWallets, walletAddress, formattedWalletAddress, walletIcon };
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@500&display=swap');
.pool {
  font-size: 1.313rem;
  font-weight: 700;
  color: white;
   /* Adjust padding for visual spacing */
}
.custom-top-bar {
  background-color: transparent;
  height: auto !important;
}

.con_wal, .disconnect_btn {
  background: rgb(109, 40, 217);
  color: white;
  border-radius: 0.25rem;
  text-transform: none;
  font-weight: 500;
  font-size: .875rem;
  line-height: 1.25rem;
  font-family: "Roboto", sans-serif;
}

.wallet-info {
  display: flex;
  align-items: center;  /* This will vertically center all items in the .wallet-info */
}

.wallet-icon {
  height: 24px;
  width: 24px;  /* Maintain aspect ratio */
  margin-right: 10px;  /* Space between the icon and the address */
}

.text_main {
  font-weight: 700;
  font-size: 1.313rem;
  color: white;
  margin-right: 15px;  /* Ensures spacing between text and button */
}
</style>