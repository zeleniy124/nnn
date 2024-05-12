<template>
    <v-col app permanent class="center sidebar" col="12" md="12">
      <div class="ml-4 pl-4 ">
        <v-row class="d-flex justify-start align-center gap-5 pb-5 pt-8 mr-0 txt">
          <img :src="logo" class="icon">
          <h1 class="logo_text">Paal AI Staking</h1>

        </v-row>

      </div>

      <RewardsBanner class="px-4 mt-5 ml-4" style="width: auto !important; max-width: none !important"/>

    </v-col>
  </template>
  
  <script>
import RewardsBanner from './RewardsBanner.vue';
  export default {
    name: 'SideBarMob',
    components : {
        RewardsBanner
    },
    data() {
      return {
        logo: require('@/assets/logo.png'),
      };
    },
  };
  </script>
  
  <style scoped>
  /* Additional styles if needed */
  .logo_text {
  color: white;
  font-size: 18px;
  font-weight: 500;
}
.icon {
  width: 2.5rem;
  height: 2.5rem;
}
.sidebar {
  background: linear-gradient(138deg,#13031F 0%,#08010B 100%);
}
.gap-5 {
  gap: 1.125rem;
}
.txt {
  border-bottom: 1px solid rgb(224 217 217 / .3); /* This sets a grey border line */
  padding-bottom: 16px; /* Adds some space below the text */
}
/* Remove the .divider class if you are not using it elsewhere */
  </style>
  