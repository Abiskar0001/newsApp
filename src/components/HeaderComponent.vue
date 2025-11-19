<template>
  <ion-header class="news-header">
    <div class="header-content">
      <div class="left-section">
        <ion-icon :icon="newspaperOutline" class="news-logo" />
        <span class="header-title">News Today</span>
      </div>

      <div class="right-section">
        <ion-button fill="clear" class="bookmark-btn" @click="goToBookmarks">
          <ion-icon :icon="bookmarkOutline" class="bookmark-icon" />
          <span class="badge" v-if="savedArticles.length > 0">{{ savedArticles.length }}</span>
        </ion-button>
      </div>
    </div>
  </ion-header>
</template>

<script setup lang="ts">
import { IonIcon, IonHeader, IonButton } from '@ionic/vue';
import { newspaperOutline, bookmarkOutline } from 'ionicons/icons';
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

interface Article {
  image: string;
  source: string;
  time: string;
  title: string;
  summary: string;
  category: string;
  url: string;
}

const savedArticles = ref<Article[]>([]);
const router = useRouter();

onMounted(() => {
  const stored = localStorage.getItem("savedArticles");
  if (stored) {
    savedArticles.value = JSON.parse(stored);
  }
});

const goToBookmarks = () => {
  router.push('/bookmarks'); 
};
</script>

<style scoped>
.news-header {
  padding: 12px 16px;
  border-bottom: 1px solid #222;
}

.header-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.left-section {
  display: flex;
  align-items: center;
  gap: 16px; 
}

.news-logo {
  font-size: 2.4rem;
  padding: 8px;
  border-radius: 14px;
  background: linear-gradient(135deg, #8A2BE2, #FF69B4);
  color: white;
  box-shadow: 0 0 10px rgba(255, 105, 180, 0.4);
}

.header-title {
  font-size: 1.8rem;
  font-weight: 700;
}

.right-section {
  display: flex;
  align-items: center;
  position: relative;
}

.bookmark-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  min-width: 52px;
  min-height: 52px;
  padding: 0 12px;
  border-radius: 16px;
  background-color: rgba(255, 255, 255, 0.05);
}

.bookmark-btn:hover {
  background-color: rgba(255, 105, 180, 0.12);
}

.bookmark-icon {
  font-size: 2.2rem;
}

.badge {
  background: linear-gradient(135deg, #FF69B4, #8A2BE2);
  color: #fff;
  font-size: 0.9rem;
  font-weight: 700;
  border-radius: 50%;
  padding: 4px 10px;      
  min-width: 24px;       
  height: 24px;           
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: -8px;
  right: -8px;
  box-shadow: 0 0 8px rgba(138, 43, 226, 0.5);
  white-space: nowrap;   
}

.bookmark-btn:hover .badge {
  transform: scale(1.1);
  transition: transform 0.2s;
}

.bookmark-btn:hover .bookmark-icon {
  transform: scale(1.1);
  transition: transform 0.2s;
}
</style>
