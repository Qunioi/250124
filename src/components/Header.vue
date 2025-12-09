<template>
  <header
    class="page-header"
    v-bind="!isFirst ? { style: { backgroundImage: `url(${getPath(`image/${themeColor}/lang/${lang}/title_${navClass}.png`)})` } } : {}"
  >
    <div class="header-top">
      <div class="header-container">
        <div class="header-logo">
          <Logo />
        </div>
        <div class="header-right">
          <div class="header-right-top">
            <div class="top-link-wrap">
              <CustomLink />
            </div>
            <Lang />
          </div>
          <div class="header-right-bottom">
            <Navigation />
          </div>
        </div>
      </div>
    </div>
    <div class="header-middle">
      <div class="header-container">
        <Member />
        <News />
      </div>
    </div>
    <div class="header-bottom" v-if="isFirst">
      <div class="header-container">
        <div class="ele-swiper-nav">
          <button class="first-swiper-btn first-swiper-prev">
            <span class="first-swiper-icon icon-prev"></span>
          </button>
          <button class="first-swiper-btn first-swiper-next">
            <span class="first-swiper-icon icon-next"></span>
          </button>
        </div>
        <div class="ele-slider-wrap">
          <div class="slider-wrap">
            <Swiper
              :modules="[Autoplay, Pagination, SwiperNavigation]"
              :pagination="false"
              :navigation="{
                nextEl: '.first-swiper-next',
                prevEl: '.first-swiper-prev'
              }"
              :slides-per-view="1"
              :loop="slides.length >= 3"
              :autoplay="slides.length >= 3 ? { delay: 3000, disableOnInteraction: false } : false"
              :allowTouchMove="false"
              @slideChange="onSlideChange">
              <SwiperSlide v-for="(slide, index) in slides" :key="slide.id">
                <img 
                  :src="getPath(`image/${themeColor}/${slide.image}`)" 
                  class="ele-slider-img"
                  crossorigin="anonymous"
                  @load="onImageLoad($event, index)">
              </SwiperSlide>
            </Swiper>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script setup>
import Logo from '@/components/common/Logo.vue';
import CustomLink from '@/components/common/CustomLink.vue';
import Lang from '@/components/common/Lang.vue';
import Member from '@/components/common/Member.vue';
import Navigation from '@/components/common/Navigation.vue';
import News from '@/components/common/News.vue';

import { getPath } from '@/composables/usePath.js';
import { useTheme } from '@/composables/useTheme.js'
import { useRoute } from 'vue-router'

const route = useRoute()

const { themeColor, lang } = useTheme() // 取得主題色與語言
const validTypes = ['card','casino','live','sport','lottery','promotion','fisharea']
const navClass = computed(() => {
  const type = route.params.type
  return validTypes.includes(type) ? type : 'welcome'
})
const isFirst = computed(() => route.meta?.pageClass?.includes('first'))

import { Swiper, SwiperSlide } from 'swiper/vue';
import { Autoplay, Pagination, Navigation as SwiperNavigation } from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/pagination';
import 'swiper/css/navigation';
import 'swiper/css/autoplay';

// 轮播图资料
const slides = ref([
  { id: 1, image: 'slider01.png' },
  { id: 2, image: 'slider02.png' }
]);

// 儲存每張圖片的背景色
const slideColors = ref([]);
const sliderBgColor = ref('transparent');

// 取得圖片右邊緣顏色
const getEdgeColor = (img) => {
  try {
    const canvas = document.createElement('canvas');
    const ctx = canvas.getContext('2d');

    canvas.width = img.naturalWidth;
    canvas.height = img.naturalHeight;
    ctx.drawImage(img, 0, 0);

    // 取最右邊 1px，垂直中間位置
    const x = canvas.width - 1;
    const y = Math.floor(canvas.height / 2);
    const pixel = ctx.getImageData(x, y, 1, 1).data;

    return `rgb(${pixel[0]}, ${pixel[1]}, ${pixel[2]})`;
  } catch (error) {
    console.warn('無法取得圖片顏色:', error);
    return 'transparent';
  }
};

// 圖片載入完成時取色
const onImageLoad = (event, index) => {
  const img = event.target;
  const color = getEdgeColor(img);
  slideColors.value[index] = color;

  // 如果是第一張圖片，立即設置背景色
  if (index === 0 && (!sliderBgColor.value || sliderBgColor.value === 'transparent')) {
    sliderBgColor.value = color;
  }
};

// Swiper 切換時更新背景色
const onSlideChange = (swiper) => {
  const currentIndex = swiper.realIndex;
  if (slideColors.value[currentIndex]) {
    sliderBgColor.value = slideColors.value[currentIndex];
  }
};

// 監聽 themeColor 變化，重置顏色資料
watch(themeColor, () => {
  slideColors.value = [];
  sliderBgColor.value = 'transparent';
}, { immediate: false });
</script>


<style lang="scss">
.header-fixed-wrap {
  position: sticky;
  z-index: 900;
}
.ele-fixed-container,
.ele-fixed-inner {
  min-width: var(--page-width);
}
.ele-fixed-inner.fixed {
  background: rgb(var(--color-primary-bg));
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 900;
}
</style>