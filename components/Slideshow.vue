<template>
  <swiper
    :modules="[SwiperAutoplay, SwiperEffectFade]"
    :autoplay="{ delay: 5000, disableOnInteraction: false }"
    :effect="'fade'"
    :loop="true"
    :speed="1000"
    :breakpoints="breakpoints"
    class="fullscreen-swiper"
    @slideChange="onSlideChange"
  >
    <swiper-slide v-for="(slide, index) in slides" :key="index" class="swiper-slide-main">
      <div 
        class="slide-content" 
        :style="{ backgroundImage: `url(${slide.image})` }"
      ></div>
    </swiper-slide>
  </swiper>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue'
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Autoplay, EffectFade } from 'swiper/modules'
import type { Swiper as SwiperType } from 'swiper'
import 'swiper/css'
import 'swiper/css/effect-fade'

type Slide = { image: string }

const SwiperAutoplay = Autoplay
const SwiperEffectFade = EffectFade

const props = defineProps<{
  slides: Slide[],
  breakpoints: Record<string, any>
}>()

const emit = defineEmits(['slideChange'])

function onSlideChange(swiper: SwiperType) {
  emit('slideChange', swiper.realIndex)
}
</script>

<style lang="scss" scoped>
.fullscreen-swiper {
  width: 100%;
  height: 100%;
}

.swiper-slide-main {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  .slide-content {
    opacity: 0;
  }
  &.swiper-slide-active {
    .slide-content {
      opacity: 1;
    }
  }
}

.slide-content {
  width: 100%;
  height: 100%;
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  
  @media (min-width: 769px) {
    max-width: calc(100vh * 16 / 9);
    max-height: 100vh;
  }

  @media (max-width: 768px) {
    max-width: 100vw;
    max-height: calc(100vw * 16 / 9);
  }
}
</style> 