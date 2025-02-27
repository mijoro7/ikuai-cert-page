<template>
  <div class="certification-page">
    <Banner :images="bannerImages" />
    <ShopInfo :shopName="shopName" :shopImage="shopImage" :shopDescription="shopDescription" />
    <AuthOptions :authMethods="authMethods" />
  </div>
</template>

<script setup lang="ts">
import Banner from './components/Banner.vue';
import ShopInfo from './components/ShopInfo.vue';
import AuthOptions from './components/AuthOptions.vue';
import { ref, computed } from 'vue';

// Data for the banner
const bannerImages = ref([
  { src: 'image/85c20cab4c10c8b9.png', alt: 'Banner Image 1' },
  { src: 'image/92e211f1b1082a5f.jpg', alt: 'Banner Image 2' },
]);

// Data for the shop information
const shopName = ref('--- WIFI ZONE ---');
const shopImage = ref('image/08c1c30306158aa0.jpg');
const shopDescription = ref('You are just one step away from accessing WIFI!');

// Data for authentication methods
const authMethods = computed(() => [
  { type: 1, label: 'User authentication', icon: 'image/users.png', gradient: 'linear-gradient(to right, #5a6cff, #36aeff)', boxShadow: '0 5px 10px rgba(70,145,255,0.5)' },
  { type: 2, label: 'Coupon Auth', icon: 'image/coupons.png', gradient: 'linear-gradient(to right, #7a5cfa, #e081e4)', boxShadow: '0 5px 10px rgba(122,92,250,0.4)' },
]);

</script>

<style scoped>
.certification-page {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 800px; /* Adjust as needed */
  margin: 0 auto;
  padding: 1rem;
  box-sizing: border-box;
}
</style>
