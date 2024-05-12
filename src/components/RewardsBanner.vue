<template>
    <v-card class="rewards-banner" dark>
      <v-card-text style="padding: 0">
        <div class="icon-wrapper">
          <!-- Your icon here -->
        </div>
        <div class="text-wrapper">
          <v-row>
              <img :src="ethIcon" alt="ETH" style="height: 32px" class="w-auto pl-2 pr-2 mb-4">
            <h3 class="mb-2 high mt-1">ETH Rewards</h3>
          </v-row>
          <p class="mb-2">Earn ETH by staking!</p>
          <div class="eth-reward ">
            <span class="eth-amount  high text-base" stlye="font-size: 1.125rem; line-height: 1.75rem">873 ETH</span>
            <span class="" style="font-weight: 500; line-height: 1.5rem; font-size: 1rem">Total ETH Rewarded</span>
          </div>
        </div>
      </v-card-text>
    </v-card>
  </template>
  
  <script>
  export default {
    name: 'RewardsBanner',
    data() {
    return {
      ethIcon: require('@/assets/eth.png'), // Use require to include your image
    }
  }
  };
  </script>
  
  <style scoped>
  .rewards-banner {
    padding-top: 10px;
    padding-bottom: 10px;
    padding-left: 16px;
    padding-left: 16px;
    background: linear-gradient(155deg,#0C0213 0%,#2A0743 100%); /* Example gradient */
    color: white;
    border-radius: 12px; /* Adjust the border-radius as per your design */
    font-size: 0.875rem;
    font-weight: 400;
    letter-spacing: 0.0178571429em;
    text-transform: none;
    box-sizing: border-box; /* Include padding in the width */
    max-width: 90%; /* Adjust max-width to the desired width or use padding */

  }
  .text-base {
    font-size: 1rem !important;
    line-height: 1.5rem;
  }
  .text-lg {
    font-size: 1.125rem !important;
    line-height: 1.75rem;
  }
  .high {
    font-weight: 700;
  }

  .icon-wrapper {
    /* Style for the icon wrapper */
    width: 17.5px;
    height: 17px;
  }
  
  .text-wrapper {
    text-align: left; /* Align text to the center */
  }
  
  .eth-amount {
    font-size: 2em; /* Larger font for the ETH amount */
    display: block; /* Makes it take up its own line */
  }
  
  /* Additional styles for the text, icon, etc. */
  </style>
  