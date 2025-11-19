<template>
  <div class="news-item">
    <img :src="article.image" alt="thumbnail" class="thumbnail" />

    <div class="content">
      <div class="meta">
        <span class="source">{{ article.source }}</span>
        <span class="time">• {{ article.time }}</span>
      </div>
      <h3 class="title">{{ article.title }}</h3>
      <p class="summary">{{ article.summary }}</p>
      <div class="actions">
        <ion-button
          fill="clear"
          size="small"
          class="button"
          @click="openArticle(article.url)"
        >
          Read Full Article
        </ion-button>
        <ion-button
  fill="clear"
  size="small"
  class="button"
  @click="$emit('toggle-save', article)"
>
  Save
</ion-button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  article: {
    image: string;
    source: string;
    time: string;
    title: string;
    summary: string;
    url: string; // <-- add url here
  };
}>();

// Open the article in a new tab
const openArticle = (url: string) => {
  if (url) {
    window.open(url, "_blank");
  } else {
    alert("Article URL not available");
  }
};
</script>

<style scoped>
.news-item {
  display: flex;
  border-left: 4px solid #8A2BE2;
  margin-bottom: 1rem;
  border-radius: 8px;
  overflow: hidden;
  width: 90%;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
}

.thumbnail {
  width: 20%;
  object-fit: cover;
  height: auto;
}

.content {
  padding: 1rem;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.meta {
  font-size: 0.9rem;
  color: #ccc;
  margin-bottom: 0.5rem;
}

.title {
  font-size: 1.2rem;
  font-weight: bold;
  margin: 0.2rem 0;
}

.summary {
  font-size: 0.95rem;
  margin-bottom: 0.5rem;
}

.actions {
  display: flex;
  gap: 0.5rem;
}

.button {
  --color: #000000;
  --background: linear-gradient(135deg, #8A2BE2, #FF69B4);
  --border-radius: 20px;
}
</style>
