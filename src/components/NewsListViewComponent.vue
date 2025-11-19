<template>
  <div class="list-container">
    <ListCardComponent
      v-for="(article, index) in articles"
      :key="index"
      :article="article"
      @toggle-save="props.toggleSaveArticle"

    />
  </div>
</template>

<script setup lang="ts">
import ListCardComponent from "@/components/ListCardComponent.vue";

const props = defineProps<{
  articles: Array<any>;
  toggleSaveArticle: (article: any) => void;
}>();


</script>

<style scoped>
.list-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
