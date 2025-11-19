<template>
  <ion-page>
    <HeaderComponent />


    <ion-content class="home-content">
      <span class="news-title">Top Headlines</span>

      <HeroSliderComponent v-if="topFive.length > 0" :slides="topFive" />

      <div class="filter-controls">
        <ion-button
          v-for="cat in categories"
          :key="cat"
          fill="outline"
          @click="selectedCategory = cat"
          :class="{ active: selectedCategory === cat }"
        >
          {{ cat }}
        </ion-button>
      </div>

      <div class="view-controls">
        <div class="view-header">
          <div class="view-info">
            <h2>Latest News</h2>
            <p>{{ filteredArticles.length }} articles found</p>
          </div>
          <div class="view-toggle">
            <ion-button fill="clear" @click="view = 'grid'" :class="{ active: view === 'grid' }">
              <ion-icon :icon="gridOutline"></ion-icon>
              <span class="button-text">Grid</span>
            </ion-button>
            <ion-button fill="clear" @click="view = 'list'" :class="{ active: view === 'list' }">
              <ion-icon :icon="listOutline"></ion-icon>
              <span class="button-text">List</span>
            </ion-button>
          </div>
        </div>
      </div>
      
    <ion-toolbar class="search-toolbar">
      <ion-searchbar
        class="search-title-bar"
        placeholder="Search Headlines"
        v-model="searchQuery"
      ></ion-searchbar>
    </ion-toolbar>
<transition name="fade" mode="out-in">
  <component
  :is="view === 'grid' ? NewsGridViewComponent : NewsListViewComponent"
  :articles="filteredArticles"
  :saved="savedArticles"
  :toggleSaveArticle="toggleSaveArticle"
/>
</transition>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue';
import HeaderComponent from '@/components/HeaderComponent.vue';
import HeroSliderComponent from '@/components/HeroSliderComponent.vue';
import NewsGridViewComponent from '@/components/NewsGridViewComponent.vue';
import NewsListViewComponent from '@/components/NewsListViewComponent.vue';
import {
  IonPage,
  IonToolbar,
  IonContent,
  IonSearchbar,
  IonButton,
  IonIcon
} from '@ionic/vue';
import { gridOutline, listOutline } from 'ionicons/icons';

const view = ref('grid');
const searchQuery = ref('');
const selectedCategory = ref('All');

const categories = ['All', 'Business', 'Entertainment', 'General', 'Health', 'Science', 'Sports', 'Technology'];

const API_KEY = import.meta.env.VITE_NEWS_API_KEY;
const BASE_URL = "https://newsapi.org/v2/top-headlines?country=us";

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

const articles = ref<Article[]>([]);
const topFive = ref<Article[]>([]); // FIXED TOP 5
const isLoading = ref(true);

const toggleSaveArticle = (article: Article) => {
  const exists = savedArticles.value.find(a => a.url === article.url);

  if (exists) {
    // remove bookmark
    savedArticles.value = savedArticles.value.filter(a => a.url !== article.url);
  } else {
    // add bookmark
    savedArticles.value.push(article);
  }

  localStorage.setItem("savedArticles", JSON.stringify(savedArticles.value));
};

const isSaved = (article: Article) => {
  return savedArticles.value.some(a => a.url === article.url);
};


const formatTimeAgo = (dateString: string) => {
  const now = new Date();
  const past = new Date(dateString);
  const seconds = Math.floor((now.getTime() - past.getTime()) / 1000);

  if (seconds < 60) return "Just now";
  const minutes = Math.floor(seconds / 60);
  if (minutes < 60) return `${minutes} minute${minutes > 1 ? "s" : ""} ago`;

  const hours = Math.floor(minutes / 60);
  if (hours < 24) return `${hours} hour${hours > 1 ? "s" : ""} ago`;

  const days = Math.floor(hours / 24);
  if (days < 7) return `${days} day${days > 1 ? "s" : ""} ago`;

  const weeks = Math.floor(days / 7);
  if (weeks < 4) return `${weeks} week${weeks > 1 ? "s" : ""} ago`;

  const months = Math.floor(days / 30);
  if (months < 12) return `${months} month${months > 1 ? "s" : ""} ago`;

  const years = Math.floor(days / 365);
  return `${years} year${years > 1 ? "s" : ""} ago`;
};

const fetchNews = async () => {
  isLoading.value = true;

  let url = `${BASE_URL}&apiKey=${API_KEY}`;

  if (selectedCategory.value !== "All") {
    url += `&category=${selectedCategory.value.toLowerCase()}`;
  }

  try {
    const res = await fetch(url);
    const data = await res.json();

    const mapped = data.articles.map((a: any) => ({
      image: a.urlToImage || "https://picsum.photos/900/500",
      source: a.source.name,
      time: formatTimeAgo(a.publishedAt),
      title: a.title,
      summary: a.description || "",
      category: selectedCategory.value !== "All" ? selectedCategory.value : "General",
      url: a.url
    }));

    articles.value = mapped;

    if (topFive.value.length === 0) {
      topFive.value = mapped.slice(0, 5);
    }

  } catch (err) {
    console.error("API Error:", err);
  } finally {
    isLoading.value = false;
  }
};

onMounted(() => {
  const stored = localStorage.getItem("savedArticles");
  if (stored) {
    savedArticles.value = JSON.parse(stored);
  }
  fetchNews();
});

watch(selectedCategory, fetchNews);

const filteredArticles = computed(() => {
  return articles.value.filter(article => {
    const matchesSearch =
      article.title.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      article.summary.toLowerCase().includes(searchQuery.value.toLowerCase());

    return matchesSearch;
  });
});
</script>

<style scoped>
.search-toolbar {
  border: #FF69B4 2px solid;
  --border:#444 2px solid;
  margin: 1rem auto;
  width: 95%;
  border-radius: 12px;
}
.view-toggle {
  display: flex;
  justify-content: center;
  gap: 1.5rem;
  margin-bottom: 18px;
  width: auto;
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
  gap: 1rem;
}

.view-controls {
  border: #FF69B4 1px solid;
  border-radius: 12px;
  padding: 1rem;
  margin: 10px auto;
  width: 95%;
}

.view-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}

.view-info h2 {
  font-size: 1.4rem;
  margin: 0;
}

.view-info p {
  font-size: 0.9rem;
  margin: 0.25rem 0 0;
}

.view-toggle ion-button {
  font-weight: 500;
  font-size: 0.95rem;
  border: 1px solid #3880ff;
  border-radius: 8px;

  gap: 6px;  
}

.view-toggle ion-button.active {
  --background: linear-gradient(135deg, #8A2BE2, #FF69B4);
  --color: #fff;
  box-shadow: 0 0 8px rgba(0, 176, 255, 0.4);
}
.view-toggle ion-button::part(native) {
  gap: 10px !important;
}

.filter-controls {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  margin: 1rem 0;
}

.filter-controls ion-button {
  --border-radius: 8px;
  --box-shadow: none;
  --border: 1px solid #444;
}

.filter-controls ion-button.active {
  --background: linear-gradient(135deg, #8A2BE2, #FF69B4);
  --color: #fff;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.25s ease;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
.button-text {
  display: inline-block;
  margin: 5px;
}
</style>
