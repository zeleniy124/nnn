<template>
  <v-card class="pool-card" dark>
    <v-card-text class="pb-0 pt-1">
      <v-row class="gap-3 flex-column">
        <v-col class="line-item">
          <span class="base bold">{{ duration }}</span>
          <span class="share">{{ ethRewardShare }}</span>
        </v-col>
        <v-col class="line-item">
          <span class="apr"><span style="font-size: 1.5rem; line-height: 2rem;">{{ apr }}</span> APR </span>
        </v-col>
        <v-col class="pl-0 pr-0 pt-0" style="padding-bottom: .375rem;">
          <div class="progress-bar"></div>
        </v-col>
        <v-col class="line-item">
          <span class="spaal">0 $PAAL/0%</span>
        </v-col>
      </v-row>
    </v-card-text>
    <v-card-actions class="px-0 pt-0 pb-0">
      <v-btn class="connect-wallet-btn non-clickable px-0"  @click="connect" v-if="alreadyConnectedWallets.length === 0" >Connect wallet</v-btn>
      <v-btn class="connect-wallet-btn px-0" @click="connect" v-else>Stake now</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import { init, useOnboard } from '@web3-onboard/vue';
import injectedModule from '@web3-onboard/injected-wallets';

export default {
  props: ['duration', 'apr', 'ethRewardShare'],
  methods: {
    connectWallet() {
      // Trigger wallet connection logic
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
}
}
</script>

<style scoped>
.pool-card {
  background-color: rgb(7, 1, 12);
  color: #fff;
  border-radius: 10px;
  padding-top: 20px;
  padding-bottom: 20px;
  padding-left: 24px;
  padding-right: 24px;
  text-align: left;
  border-color: rgb(224 217 217 / .3);
  border-width: 1px;
}

.line-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0;
  padding-bottom: .75rem;
}

.days,
.reward-share {
  font-size: 1rem;
}

.progress-bar {
  background: #40006f;
  height: .625rem;
  border-radius: 10px;
  width: 100%;
  padding: 0;
  
}

.connect-wallet-btn {
  background: rgb(109, 40, 217);
    color: white; /* Text color */
    border-radius: 0.25rem; /* Adjust as needed for rounded corners */
    text-transform: none; /* Prevents Vuetify from making the text uppercase */
    font-weight: 500; /* Adjust the font weight as needed */
    font-size: .875rem;
    line-height: 1.25rem;
    font-family: "Roboto", sans-serif;
    width: 100%;
}

.connect-wallet-btn:hover {
  background-color: #9d50bb;
}
.base {
  font-weight: 700;
  font-size: 1rem;
  line-height: 1.5rem;
}
.connect-wallet-btn.non-clickable {
  pointer-events: none;  /* Prevents all click, state and hover interactions */
  opacity: 0.5;  /* Optional: visually indicate the button is inactive */
}
.share {
  font-size: .875rem;
  line-height: 1.25rem;
  font-weight: 500;
  color: rgb(255 255 255 / .75);
}
.apr {
  font-size: 1rem;
  line-height: 1.5rem;
  font-weight: 700;
  color: rgb(207 125 253 )
}
.spaal {
    font-size: 0.875rem;
    font-weight: 400;
    letter-spacing: 0.0178571429em;
    text-transform: none;
}
</style>
