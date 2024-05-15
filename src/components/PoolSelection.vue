<template>
  <v-col class="gp pa-0 gp-1">
    <h1 class="pool pt-4 pl-4">Staking</h1>
    <h1 class="pool pt-4 pl-4">Pool size</h1>
    <h2 class="desc pl-4">Leverage the power of compounding by staking your $PAAL tokens and compounding rewards as they accrue.</h2>
  </v-col>
  <v-row class="justify-space-between">
    <v-col
      cols="12"
      md="4"
      v-for="pool in pools"
      :key="pool.id"
    >
      <PoolCard 
        :duration="pool.duration" 
        :apr="pool.apr" 
        :ethRewardShare="pool.ethRewardShare"
      />
    </v-col>
  </v-row>
</template>

<script>
import PoolCard from './PoolCard.vue';

export default {
  components: {
    PoolCard
  },
  data() {
    return {
      pools: [
        { id: 1, duration: "14 Days", apr: "3%", ethRewardShare: "1.20% ETH Rewards Share" },
        { id: 2, duration: "28 Days", apr: "10%", ethRewardShare: "6% ETH Rewards Share" },
        { id: 3, duration: "56 Days", apr: "15%", ethRewardShare: "92.8% ETH Rewards Share" }
        // More pools can be added here
      ]
    }
  }
}
</script>
<style>
.gp {
  margin-top: 2.5rem;
}
.desc {
  font-size: 1.125rem;
  line-height: 1.75rem;
  font-weight: 500;
  color: white;
}
.pool {
  font-size: 1.313rem;
  font-weight: 700;
  color: white;
}
.gp-1 {
  margin-bottom: 1.25rem;
}
</style>