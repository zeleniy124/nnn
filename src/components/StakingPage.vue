<template>
    <v-container>
      <RewardsBanner />
      <PoolSelection />
      <MyStakes />
    </v-container>
  </template>
  
  <script>
  import RewardsBanner from './RewardsBanner.vue';
  import PoolSelection from './PoolSelection.vue';
  import MyStakes from './MyStakes.vue';
  
  export default {
    components: {
      RewardsBanner,
      PoolSelection,
      MyStakes
    }
  }
  </script>
  