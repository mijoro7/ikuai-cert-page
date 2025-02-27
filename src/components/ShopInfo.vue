<template>
  <div class="shop-info">
    <img :src="shopImage" :alt="shopName" class="shop-image" />
    <div class="shop-details">
      <div class="shop-name">{{ shopName }}</div>
      <p class="shop-description">{{ shopDescription }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';

const props = defineProps<{
  shopName: string;
  shopImage: string;
  shopDescription: string;
}>();
</script>

<style scoped>
.shop-info {
  display: flex;
  align-items: center;
  padding: 1rem;
  border-bottom: 2px solid #f8f8f8;
  width: 100%;
  box-sizing: border-box;
}

.shop-image {
  width: 4rem;
  height: 4rem;
  border-radius: 50%;
  margin-right: 1rem;
}

.shop-details {
  flex-grow: 1;
  text-align: left;
}

.shop-name {
  font-size: 1.2rem;
  font-weight: bold;
  color: #333;
  margin-bottom: 0.5rem;
}

.shop-description {
  font-size: 1rem;
  color: #666;
}
</style>
