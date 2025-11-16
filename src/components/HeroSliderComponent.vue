<template>
  <div class="slider-container">
    <swiper
      :modules="[Autoplay]"
      :slides-per-view="1"
      :loop="true"
      :autoplay="{ delay: 4000, disableOnInteraction: false }"
      class="hero-slider"
      @swiper="onSwiperInit"
    >
      <swiper-slide v-for="(item, index) in slides" :key="item.id">
        <div class="slide">
          <img :src="item.image" :alt="item.title" class="slide-image" />
          <div class="slide-overlay">
            <h2 class="headline">{{ item.title }}</h2>
            <p class="subtext">{{ item.description }}</p>
            <div class="meta">
              <span class="source">{{ item.source }}</span>
              <span class="time">{{ item.time }}</span>
            </div>
            <button class="read-button">Read Full Story</button>
          </div>
        </div>
      </swiper-slide>
    </swiper>

    <!-- Custom dot indicators -->
    <div class="slide-dots">
      <span
        v-for="(item, index) in slides"
        :key="index"
        :class="['dot', { active: currentSlide === index }]"
        @click="goToSlide(index)"
      ></span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import { Autoplay } from 'swiper/modules';
import 'swiper/css';

const currentSlide = ref(0);
let swiperInstance: any = null;

const slides = [
  {
    id: 1,
    title: 'Major Breakthrough in Renewable Energy Technology Announced',
    description: 'Scientists have developed a new solar panel technology that promises to double energy efficiency.',
    source: 'Tech Daily',
    time: '2h ago',
    image: 'https://picsum.photos/900/500?random=2'
  },
  {
    id: 2,
    title: 'AI Revolutionizes Medical Diagnostics',
    description: 'New AI tools are helping doctors detect diseases earlier and more accurately than ever before.',
    source: 'Health Weekly',
    time: '5h ago',
    image: 'https://picsum.photos/900/500?random=3'
  },
  {
    id: 3,
    title: 'SpaceX Launches Next-Gen Satellite Network',
    description: 'The new constellation aims to provide high-speed internet to remote regions worldwide.',
    source: 'Orbit News',
    time: '3h ago',
    image: 'https://picsum.photos/900/500?random=4'
  }
];

const onSwiperInit = (swiper: any) => {
  swiperInstance = swiper;
  currentSlide.value = swiper.realIndex;
  swiper.on('slideChange', () => {
    currentSlide.value = swiper.realIndex;
  });
};

const goToSlide = (index: number) => {
  if (swiperInstance) swiperInstance.slideToLoop(index);
};
</script>

<style scoped>
.slider-container {
  position: relative;
}

.hero-slider {
  width: 95%;          
  max-width: 100%;     
  margin: 1rem auto;
  height: 400px;        /* Adjust as needed */
}

.slide {
  position: relative;
  width: 100%;
  height: 100%;
  border-radius: 12px;
  overflow: hidden;
}

.slide-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}


.slide-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 1rem;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.8), transparent);
  color: white;
}

.headline {
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 0.3rem;
}

.subtext {
  font-size: 0.9rem;
  color: #ccc;
  margin-bottom: 0.5rem;
}

.meta {
  font-size: 0.8rem;
  color: #aaa;
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.5rem;
}

.read-button {
  background: linear-gradient(135deg, #8A2BE2, #FF69B4);
  color: white;
  border: none;
  padding: 0.4rem 0.8rem;
  border-radius: 50px;
  font-weight: bold;
  cursor: pointer;
}

/* Dot indicators */
.slide-dots {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  margin-top: 1rem;
}

.dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #aaa;
  cursor: pointer;
  transition: all 0.3s ease;
}

.dot.active {
  width: 20px; /* dash for active slide */
  border-radius: 5px;
  background: linear-gradient(135deg, #8A2BE2, #FF69B4);
}
</style>
