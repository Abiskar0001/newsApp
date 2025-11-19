<template>
    <ion-page>
        <HeaderComponent />

        <ion-content class="home-content">
            <div class="bookmarks-header">
                <h2 class="news-title">Your Bookmarked Articles</h2>
                <ion-button @click="goHome" fill="outline">Go Home</ion-button>
            </div>

            <component
                :is="view === 'grid' ? NewsGridViewComponent : NewsListViewComponent"
                :articles="savedArticles"
                :saved="savedArticles"
                :toggleSaveArticle="toggleSaveArticle"
            />
        </ion-content>
    </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import HeaderComponent from '@/components/HeaderComponent.vue';
import NewsGridViewComponent from '@/components/NewsGridViewComponent.vue';
import NewsListViewComponent from '@/components/NewsListViewComponent.vue';
import { IonPage, IonContent, IonButton } from '@ionic/vue';

const router = useRouter();
const goHome = () => {
    router.push('/');
};

const view = ref('grid');
const savedArticles = ref<any[]>([]);

onMounted(() => {
    const stored = localStorage.getItem("savedArticles");
    if (stored) {
        savedArticles.value = JSON.parse(stored);
    }
});

const toggleSaveArticle = (article: any) => {
    const exists = savedArticles.value.find((a: any) => a.url === article.url);
    if (exists) {
        savedArticles.value = savedArticles.value.filter((a: any) => a.url !== article.url);
    } else {
        savedArticles.value.push(article);
    }
    localStorage.setItem("savedArticles", JSON.stringify(savedArticles.value));
};
</script>

<style scoped>
.news-title {
    padding: 20px;
font-weight: 600;
}
.bookmarks-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 16px;
    margin: 16px 0;
}
</style>