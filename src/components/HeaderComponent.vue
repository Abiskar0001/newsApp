<template>
  <ion-header>
      <ion-title>
        <ion-icon :icon="newspaperOutline" class="news-logo" />
        News Today
      </ion-title>

  </ion-header>
</template>

<script setup lang="ts">
import { IonIcon, IonHeader,IonTitle } from '@ionic/vue';
import { newspaperOutline } from 'ionicons/icons';
</script>

<style scoped>
ion-title {
  display: flex;
  align-items: center;
  height: 60px;
}

.news-logo {
  margin-right: 10px;
  vertical-align: middle;
  font-size: 2rem;
  padding: 8px;
  border-radius: 12px;
  background: linear-gradient(135deg, #8A2BE2, #FF69B4);
  color: white;
}

.search-toolbar {
  padding: 0;
  height: auto;
}

.search-title-bar {
  margin: 0 16px;
  width: calc(100% - 32px);
}

.search-title-bar::part(input) {
  background-color: #f0f0f0;
  border-radius: 100px;
}
</style>