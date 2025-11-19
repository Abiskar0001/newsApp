<template>
  <ion-card class="news-card">
    <ion-img :src="article.image" />

    <ion-card-header>
      <ion-card-subtitle>
        {{ article.source }} • {{ article.time }}
      </ion-card-subtitle>
      <ion-card-title>{{ article.title }}</ion-card-title>
    </ion-card-header>

    <ion-card-content>
      <p>{{ article.summary }}</p>

      <div class="card-actions">
        <ion-button
          fill="clear"
          size="small"
          class="button"
          @click="openArticle"
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
    </ion-card-content>
  </ion-card>
</template>

<script setup lang="ts">
const props = defineProps<{
  article: {
    image: string;
    source: string;
    time: string;
    title: string;
    summary: string;
    url: string;
  };
}>();

const openArticle = async () => {
  if (!props.article.url) return;

  const anyWin = window as any;
  try {
    const pluginBrowser = anyWin.Capacitor?.Plugins?.Browser || anyWin.Capacitor?.Browser || anyWin.Capacitor?.Plugins?.InAppBrowser;
    if (pluginBrowser && typeof pluginBrowser.open === 'function') {
      await pluginBrowser.open({ url: props.article.url });
      return;
    }

    const directBrowser = anyWin.Capacitor?.Plugins?.Browser;
    if (directBrowser && typeof directBrowser.open === 'function') {
      await directBrowser.open({ url: props.article.url });
      return;
    }
  } catch (e) {
    // ignore and fallback
  }

  // Fallback for web
  window.open(props.article.url, '_blank');
};
</script>

<style scoped>
.news-card {
  margin: 0.5rem;
  border-radius: 8px;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.news-card::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 6px;
  background: linear-gradient(135deg, #8A2BE2, #FF69B4);
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.card-actions {
  display: flex;
  padding: 5px 0;
  margin-top: auto;
  justify-content: flex-end;
  gap: 0.5rem;
}

.button {
  --color: #000000;
  --background: linear-gradient(135deg, #8A2BE2, #FF69B4);
  --border-radius: 20px;
}

ion-card-content {
  display: flex;
  flex-direction: column;
  flex: 1;
}
</style>
