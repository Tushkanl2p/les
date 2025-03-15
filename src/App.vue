<script setup lang="ts"></script>

<template>
  <body>
    <swiper
      :direction="'vertical'"
      :slidesPerView="1"
      :spaceBetween="0"
      :parallax="true"
      :mousewheel="true"
      :speed="1700"
      :pagination="{
        clickable: true
      }"
      :modules="modules"
      class="mySwiper"
      @slideChange="onSlideChange"
    >
      <div class="slider-ui">
        <div class="container header-wrapper">
          <div class="top-line">
            <div class="row">
              <div class="col col--center">
                <a href="#" class="logo">
                  <img src="/src/assets/img/logo.svg" />
                </a>
              </div>
              <div class="col col--center col--right col--lead">
                <nav class="main-menu">
                  <ul>
                    <li><a href="#">Main</a></li>
                    <li class="active"><span>Info</span></li>
                    <li><a href="#">Contact</a></li>
                  </ul>
                </nav>
                <a href="#" class="button button--top">Plant now</a>
              </div>
              <div class="col col--center">
                <button
                  class="submenu anim-menu-btn"
                  :class="{ 'anim-menu-btn--state-b': isActive }"
                  @click="toggleButton"
                >
                  <i class="anim-menu-btn__icon anim-menu-btn__icon--close"></i>
                </button>
              </div>
            </div>
          </div>
          <div class="header-content">
            <div
              class="header-content__slide"
              v-for="(slide, index) in slides"
              :key="'header-' + index"
            >
              <h1 class="letters">{{ slide.title }}</h1>
              <div class="header-content__info">
                <p>{{ slide.description }}</p>
                <br />
              </div>
            </div>
          </div>
        </div>
      </div>

      <swiper-slide class="slider__item" v-for="(slide, index) in slides" :key="'swiper-' + index">
        <div
          class="slider__layer"
          data-swiper-parallax="35%"
          :style="{ backgroundImage: `url(${slide.layers.background})` }"
        ></div>
        <div class="slider__layer" data-swiper-parallax="25%">
          <img :src="slide.layers.middle" alt="Middle layer" />
        </div>
        <div class="slider__layer" data-swiper-parallax="14%">
          <img :src="slide.layers.front" alt="Front layer" /></div
      ></swiper-slide>
    </swiper>
  </body>
</template>
<script setup>
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css'
import 'swiper/css/pagination'
import { Parallax, Mousewheel, Pagination } from 'swiper/modules'
import { ref, onMounted } from 'vue'

const slides = ref([
  {
    title: 'Grow',
    description:
      'As an environmental charity, we’re on a mission to make it simple for everyone to help the environment by planting each one tree!',
    layers: {
      background: '/src/assets/img/slides/slide-1-layer-back.jpg',
      middle: '/src/assets/img/slides/slide-1-layer-middle.png',
      front: '/src/assets/img/slides/slide-1-layer-front.png'
    }
  },
  {
    title: 'Live',
    description:
      'As an environmental charity, we’re on a mission to make it simple for everyone to help the environment by planting each one tree!',
    layers: {
      background: '/src/assets/img/slides/slide-2-layer-back.jpg',
      middle: '/src/assets/img/slides/slide-2-layer-middle.png',
      front: '/src/assets/img/slides/slide-2-layer-front.png'
    }
  },
  {
    title: 'Weed',
    description:
      'As an environmental charity, we’re on a mission to make it simple for everyone to help the environment by planting each one tree!',
    layers: {
      background: '/src/assets/img/slides/slide-3-layer-back.jpg',
      middle: '/src/assets/img/slides/slide-3-layer-middle.png',
      front: '/src/assets/img/slides/slide-3-layer-front.png'
    }
  }
])

const isActive = ref(false)

const toggleButton = () => {
  isActive.value = !isActive.value

  const event = new CustomEvent('anim-menu-btn-clicked', {
    detail: isActive.value
  })

  document.dispatchEvent(event)
}

const modules = [Parallax, Mousewheel, Pagination]

const updateActiveSlide = (activeIndex) => {
  const slidesElements = document.querySelectorAll('.header-content__slide')
  slidesElements.forEach((slide, index) => {
    slide.classList.toggle('active', index === activeIndex)
  })
}

const onSlideChange = (swiper) => {
  updateActiveSlide(swiper.activeIndex)
}

onMounted(() => {
  updateActiveSlide(0)

  document.querySelectorAll('.header-content h1').forEach((element) => {
    element.innerHTML = element.textContent
      .replace(/ (-|#|@){1}/g, (s) => s[1] + s[0])
      .replace(/(\S*)/g, (match) => {
        return match.replace(/\S(-|#|@)?/g, '<span class="letter">$&</span>')
      })

    element.querySelectorAll('.letter').forEach((letter, index) => {
      letter.setAttribute('style', `z-index: -${index}; transition-duration: ${index / 5 + 1}s`)
    })
  })
})
</script>

<style>
@import '../src/assets/libs/btn/anim-menu-btn.css';
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  height: 100%;
}
template,
body {
  height: 100%;
  background-color: #2b474b;
  color: var(--white-color);
  overflow: hidden;
}
@font-face {
  font-family: montserrat-g;
  src: url(../src/assets/fonts/Montserrat-Regular.woff2);
}

@font-face {
  font-family: montserrat-g;
  src: url(../src/assets/fonts/Montserrat-Bold.woff2);
  font-weight: 700;
}
@font-face {
  font-family: montserrat-g;
  src: url(../src/assets/fonts/Montserrat-Black.woff2);
  font-weight: 900;
}
@font-face {
  font-family: montserrat-g;
  src: url(../src/assets/fonts/Montserrat-SemiBold.woff2);
  font-weight: 600;
}

body {
  font-size: 16px;
  font-family: montserrat-g, sans-serif;
}

.mySwiper {
  width: 100%;
  height: 100vh;
}
.slider__layer {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  will-change: transform;
  width: 100%;
  height: 100%;
}

.slider__item {
  overflow: hidden;
}

.slider-ui {
  position: absolute;
  z-index: 10;
  inset: 0;
}
:root {
  --grid-width: 1140px;
  --grid-gutter: 0.5em;
}
.container {
  max-width: calc(var(--grid-width) + var(--grid-gutter) * 2);
  min-width: calc(320px + var(--grid-gutter) * 2);
  padding: 0 var(--grid-gutter);
  margin: auto;
}
.row {
  display: flex;
  height: auto;
}
.col {
  width: fit-content;
  padding: 0 var(--grid-gutter);
}
.col--lead {
  flex: 1;
}
.col--center {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  height: 100%;
}
.col--right {
  display: flex;
  justify-content: right;
}
.top-line {
  padding: 3.2em 0;
}
.logo {
  display: block;
}

.logo img {
  display: block;
  width: 15.5em;
}
.main-menu li {
  list-style-type: none;
  display: inline;
}
.main-menu li > * {
  color: var(--gray-color);
  text-transform: uppercase;
  font-weight: 700;
  text-decoration: none;
  font-size: 0.725em;
  letter-spacing: 0.25em;
  position: relative;
}
.main-menu li > *::before,
.main-menu li > *::after {
  content: '';
  display: inline-block;
  height: 2px;
  background-color: var(--green-color);
  width: 11.25px;
  vertical-align: middle;
  top: -1px;
  left: -1.5px;
  position: relative;
  border-radius: 10em;
  margin: 0 8.25px;
  visibility: hidden;
}
.main-menu li.active > *,
.main-menu li:hover > * {
  color: var(--white-color);
}
.main-menu li.active > *::before,
.main-menu li:hover > *::before,
.main-menu li.active > *::after,
.main-menu li:hover > *::after {
  visibility: visible;
}
:root {
  --white-color: #fff;
  --gray-color: #aeb4a6;
  --green-color: #94f515;
  --index: calc(1vw + 1vh);
  --transition: ease-in-out;
}
.button {
  padding: 1.05em 2.6em;
  color: var(--white-color);
  background-color: var(--green-color);
  border: 1.5px solid transparent;
  border-radius: 10em 0 10em 10em;
  font-weight: 600;
  text-decoration: none;
  letter-spacing: 0.21em;
  text-transform: uppercase;
  font-size: 0.65em;
  cursor: pointer;
}
.button--top {
  background-color: transparent;
  border-color: var(--green-color);
  margin: 0 2em 0 3em;
}
.button:hover {
  background-color: var(--green-color);
}
.submenu {
  --anim-menu-btn-color: var(--gray-color);
  --anim-menu-btn-gap: 0.195em;
  --anim-menu-btn-width: 1.1em;
}
.header-wrapper {
  display: flex;
  flex-direction: column;
  height: 100%;
}
.header-content {
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  line-height: 1;
  flex-direction: column;
}
.header-content__slide {
  position: absolute;
}

.header-content h1 {
  font-size: calc(var(--index) * 11.95);
  text-transform: uppercase;
  font-weight: 900;
}

.header-content p {
  max-width: 560px;
  font-weight: 600;
  line-height: 1.4;
  display: inline-block;
}
.header-bottom {
  min-height: 120px;
}
.letters .letter {
  font-weight: 900;
  text-shadow: 25px 0 35px rgba(0 0 0 /0.4);
  position: relative;
  margin-left: calc(var(--index) * -2.65);
  right: calc(var(--index) * -2.65);
  opacity: 0;
  top: -5em;
  transition:
    opacity var(--transition),
    top var(--transition);
}
.header-content__slide.active .letter {
  opacity: 1;
  top: 0;
}

.header-content__slide.active + * .letter {
  top: 5em;
}

.slider__layer img {
  max-width: 100%;
  height: auto;
  object-fit: contain;
}
.header-content__info {
  top: -37em;
  position: relative;
  opacity: 0;
  transition:
    opacity 1s ease,
    top 1.25s ease;
}
.header-content__slide.active .header-content__info {
  opacity: 1;
  top: 1;
}
.header-content__info p {
  margin: 0;
  font-weight: 600;
  line-height: 1.4;
}
</style>
