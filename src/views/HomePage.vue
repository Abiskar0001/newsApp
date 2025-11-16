<template>
  <ion-page>
    <HeaderComponent />
    <ion-toolbar class="search-toolbar">
      <ion-searchbar class="search-title-bar" placeholder="Search Headlines"></ion-searchbar>
    </ion-toolbar>

    <ion-content class="home-content">
      <span class="news-title">Top Headlines</span>
      <HeroSliderComponent />
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent
} from '@ionic/vue';
import HeroSliderComponent from '@/components/HeroSliderComponent.vue';
import HeaderComponent from '@/components/HeaderComponent.vue';
</script>

<style scoped>
.news-header {
  --background: linear-gradient(135deg, #8A2BE2, #FF69B4);
}

.news-title {
  font-size: 1.8rem;
  font-weight: bold;
  display: block;
  padding: 1rem 0;
  text-align: center;
}

.home-content {
  background: #0f0f0f;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  height: 100%;
}
</style>