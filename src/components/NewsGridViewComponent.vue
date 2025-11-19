<template>
  <div class="grid-container">
    <NewsCardComponent
  v-for="article in articles"
  :key="article.url"
  :article="article"
  @toggle-save="toggleSaveArticle"
/>
  </div>
</template>

<script setup lang="ts">
import NewsCardComponent from './NewsCardComponent.vue';

const props = defineProps<{
  articles: Array<any>;
  toggleSaveArticle: (article: any) => void;
}>();


</script>

<style scoped>
.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1rem;
  width: 90%;
  margin: 0 auto;
}
</style>
