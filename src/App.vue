<template>
  <v-app class="cont" :style="backgroundStyle">
    <!-- Main content area -->
    <v-main class="p-0">
      <!-- Provides a container for your application components -->
      <v-container class="ml-0 mr-0 cont pl-0 pt-0">
        <v-row>
          <!-- Sidebar displayed on large screens (md and up) -->
          <v-col v-if="isLargeScreen" cols="12" md="3">
            <SideBar/>
          </v-col>
          <!-- Main Content: adjust cols based on sidebar visibility -->
          <SideBarMob v-if="!isLargeScreen" style="" class=""/>
          <v-col cols="12" :class="paddingClass" >
            <!-- Top Bar displayed on small screens (sm and down) -->
            <TopBar v-if="isLargeScreen"/>
            <StackReward class="pl-0"/>
            <PoolSelection style="width: auto"/>
            <MyStakes class="gp"/>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import SideBar from './components/Sidebar.vue';
import TopBar from './components/TopBar.vue';
import PoolSelection from './components/PoolSelection.vue';
import MyStakes from './components/MyStakes.vue';
import SideBarMob from './components/SideBarMob.vue'
import StackReward from './components/RewardsStacking.vue'
export default {
  name: 'App',
  components: {
    TopBar, SideBar, PoolSelection, MyStakes, SideBarMob, StackReward
  },
  data() {
    return {
      isLargeScreen: false, // Controls the visibility based on screen width
      bg: require('@/assets/bg.png'),
    };
  },
  computed: {
    backgroundStyle() {
      return {
        backgroundImage: `linear-gradient(0deg, rgba(6, 0, 6, 0.85), rgba(6, 0, 6, 0.85)), url(${this.bg})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center center',
        backgroundRepeat: 'no-repeat',
      };
    },
    paddingClass() {
      return this.isLargeScreen ? 'pa-8' : 'pa-4 pl-8';
    }
  },
  mounted() {
    this.handleResize(); // Check screen size on mount
    window.addEventListener('resize', this.handleResize); // Adjust on window resize
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.handleResize); // Cleanup listener on component destroy
  },
  methods: {
    handleResize() {
      this.isLargeScreen = window.innerWidth >= 960; // Toggle based on this breakpoint
    }
  }
};
</script>
