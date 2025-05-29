<template>
  <div 
    class="landing-page"
  >
    <div 
      class="background-blur"
      :style="{ backgroundImage: `url(${currentSlides[currentSlideIndex].image})` }"
    ></div>
    <div class="aspect-ratio-container">
      <div class="content-container">
        <Slideshow :slides="currentSlides" :breakpoints="swiperBreakpoints" @slideChange="handleSlideChange" />

        <div class="button-toggle" v-if="isMobile" @click="showMenu = !showMenu">
          <span :class="{'bar': true, 'open': showMenu}"></span>
          <span :class="{'bar': true, 'open': showMenu}"></span>
          <span :class="{'bar': true, 'open': showMenu}"></span>
        </div>
        <div 
          class="button-container" 
          :class="{ 'show': !isMobile || showMenu, 'hide': isMobile && !showMenu }"
        >
          <a href="https://www.performing-arts.gov.hk/en/progamme-e-registration.html?group=0315000000000020" target="_blank" rel="noopener noreferrer">
            <button class="scribble-button subscribe-button">
              <span class="button-text">
                <span class="text-zh">訂閱「亞藝無疆」通訊</span>
                <span class="text-en">Subscribe Asia+ Newsletter</span>
              </span>
            </button>
          </a>
          
          <a href="https://www.asiaplus.gov.hk" target="_blank" rel="noopener noreferrer">
            <button class="scribble-button programme-button">
              <span class="button-text">
                <span class="text-zh">回顧亞藝無疆2024精彩節目</span>
                <span class="text-en">Asia+ 2024 Programmes</span>
              </span>
            </button>
          </a>

          <div class="social-icons">
            <a href="https://www.facebook.com/asiaplusfestival/" target="_blank" rel="noopener noreferrer" class="social-icon facebook">
              <font-awesome-icon :icon="['fab', 'facebook-f']" />
            </a>
            <a href="https://www.instagram.com/asiaplusfestival/" target="_blank" rel="noopener noreferrer" class="social-icon instagram">
              <font-awesome-icon :icon="['fab', 'instagram']" />
            </a>
            <a href="https://www.youtube.com/@asiaplusfestival" target="_blank" rel="noopener noreferrer" class="social-icon youtube">
              <font-awesome-icon :icon="['fab', 'youtube']" />
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import Slideshow from './Slideshow.vue'

const swiperBreakpoints = {
  320: {
    slidesPerView: 1,
  },
  768: {
    slidesPerView: 1,
  },
  1024: {
    slidesPerView: 1,
  }
}

// Desktop slides
const desktopSlides = [
  { 
    image: "/images/s1.jpg"
  },
  { 
    image: "/images/s2.jpg"
  },
  {
    image: "/images/s3.jpg"
  },
  {
    image: "/images/s4.jpg"
  }
]

// Mobile slides
const mobileSlides = [
  { 
    image: "/images/s1-mobile.jpg"
  },
  { 
    image: "/images/s2-mobile.jpg"
  },
  {
    image: "/images/s3-mobile.jpg"
  },
  {
    image: "/images/s4-mobile.jpg"
  }
]

const windowWidth = ref(window.innerWidth)
const isMobile = computed(() => windowWidth.value <= 768)
const currentSlideIndex = ref(0)
const showMenu = ref(false)

const currentSlides = computed(() => {
  return isMobile.value ? mobileSlides : desktopSlides
})

const handleSlideChange = (index: number) => {
  currentSlideIndex.value = index
}

const handleResize = () => {
  windowWidth.value = window.innerWidth
}

onMounted(() => {
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
})
</script>

<style lang="scss" scoped>
// Variables
$button-container-width-desktop: 600px;
$button-container-width-mobile: 320px;
$button-padding-desktop: clamp(0.8rem, 2vw, 1rem) clamp(1.5rem, 3vw, 2rem);
$button-padding-mobile: 0.8rem 1.5rem;
$button-font-size-desktop: clamp(1rem, 2vw, 1.1rem);
$button-font-size-mobile: 0.9rem;
$social-icon-size-desktop: 2.5rem;
$social-icon-size-mobile: 2rem;

// Mixins
@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

@mixin button-hover {
  transition: transform 0.3s ease;
  &:hover {
    transform: scale(1.05);
  }
}

.landing-page {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  @include flex-center;
}

.background-blur {
  position: fixed;
  inset: 0;
  background-size: cover;
  background-position: center;
  filter: blur(20px) brightness(0.8);
  transform: scale(1.1);
  transition: background-image 1s ease;
  z-index: -1;
}

.aspect-ratio-container {
  width: 100%;
  height: 100%;
  position: relative;
  overflow: hidden;
  z-index: 1;
  @include flex-center;
}

.content-container {
  position: relative;
  width: 100%;
  height: 100%;
  @include flex-center;
}

.button-toggle {
  position: fixed;
  bottom: var(--page-padding);
  right: var(--page-padding);
  z-index: 20;
  width: 48px;
  height: 48px;
  background: rgba(194, 52, 98, 0.92);
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
  @media (min-width: 769px) {
    display: none;
  }
  .bar {
    display: block;
    width: 26px;
    height: 3px;
    margin: 3px 0;
    background: #fff;
    border-radius: 2px;
    transition: 0.3s;
  }
  & .bar.open:nth-child(1) {
    transform: translateY(6px) rotate(45deg);
  }
  & .bar.open:nth-child(2) {
    opacity: 0;
  }
  & .bar.open:nth-child(3) {
    transform: translateY(-6px) rotate(-45deg);
  }
}

.button-container {
  position: fixed;
  bottom: var(--page-padding);
  right: var(--page-padding);
  z-index: 10;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: $button-container-width-desktop;
  transition: opacity 0.3s, visibility 0.3s, transform 0.3s;

  @media (max-width: 768px) {
    bottom: var(--page-padding);
    right: var(--page-padding);
    gap: 0.8rem;
    width: 100svw;
    max-width: $button-container-width-mobile;
  }

  &.hide {
    opacity: 0;
    visibility: hidden;
    pointer-events: none;
    transform: translateY(20px);
  }
  &.show {
    opacity: 1;
    visibility: visible;
    pointer-events: auto;
    transform: translateY(0);
  }
}

.scribble-button {
  background: var(--button-bg-color);
  color: var(--button-text-color);
  border: none;
  padding: $button-padding-desktop;
  border-radius: 100px;
  font-size: $button-font-size-desktop;
  cursor: pointer;
  text-align: center;
  line-height: 1.4;
  width: 100%;
  font-weight: 600;
  @include button-hover;

  &.programme-button {
    border: 2px solid var(--button-bg-color);
  }

  @media (max-width: 768px) {
    padding: $button-padding-mobile;
    font-size: $button-font-size-mobile;
  }
}

.button-text {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;

  .text-zh,
  .text-en {
    font-size: 1em;

    @media (max-width: 768px) {
      font-size: 0.85em;
    }
  }
}

.social-icons {
  display: flex;
  justify-content: center;
  gap: 1.5rem;
  margin-top: .2rem;

  @media (max-width: 768px) {
    gap: 1rem;
    margin-top: 0.8rem;
  }
}

.social-icon {
  color: white;
  font-size: 1.5rem;
  @include flex-center;
  @include button-hover;
  width: $social-icon-size-desktop;
  height: $social-icon-size-desktop;
  border-radius: 50%;

  &.facebook {
    background-color: var(--facebook-color);
  }

  &.instagram {
    background-color: var(--instagram-color);
  }

  &.youtube {
    background-color: var(--youtube-color);
  }

  @media (max-width: 768px) {
    font-size: 1.2rem;
    width: $social-icon-size-mobile;
    height: $social-icon-size-mobile;
  }
}
</style> 