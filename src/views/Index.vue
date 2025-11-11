<template>
  <div class="page-container">
    <!-- <FloatImg /> -->
    <section class="first-section-wrap first-slider-wrap">
      <!-- 轮播 -->
      <div class="ele-slider-wrap slider-wrap">
        <Swiper
          :modules="[Autoplay, Pagination]"
          :pagination="{ clickable: true }"
          :slides-per-view="1"
          :loop="true"
          :allowTouchMove="false">
          <SwiperSlide v-for="(slide, index) in slides" :key="slide.id">
            <img :src="getPath(`image/${themeColor}/${slide.image}`)" class="ele-slider-img" />
          </SwiperSlide>
        </Swiper>
      </div>
      <div class="section-wrap">
        <div class="first-website-wrap">
          <div class="first-website-inner">
            <div class="website-item">
              <div class="website-item-icon website-item-icon--deposit"></div>
              <div class="website-item-title">存款平均<br>到帐时间</div>
              <div class="website-item-num"><span>20</span>秒</div>
            </div>
            <div class="website-item">
              <div class="website-item-icon website-item-icon--withdraw"></div>
              <div class="website-item-title">取款平均<br>到帐时间</div>
              <div class="website-item-num"><span>20</span>秒</div>
            </div>
            <div class="website-item">
              <div class="website-item-icon website-item-icon--platform"></div>
              <div class="website-item-title">官方合作<br>游戏平台</div>
              <div class="website-item-num"><span>50</span>家</div>
            </div>
            <div class="website-item">
              <div class="website-item-icon website-item-icon--bank"></div>
              <div class="website-item-title">银行服务<br>合作平台</div>
              <div class="website-item-num"><span>30</span>家</div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <section class="first-section-wrap first-hotgame-wrap">
      <div class="section-wrap">
        <div class="first-hotgame-header">
          <div class="first-hotgame-title">
            <img :src="getPath(`image/lang/${lang}/first_hotgame_title.png`)" />
          </div>
          <button class="first-hotgame-more-btn">
            <span>More</span>
            <svg xmlns="http://www.w3.org/2000/svg" width="21" height="21" viewBox="0 0 512 512" fill="currentColor">
              <path
                d="M0 256a256 256 0 1 0 512 0A256 256 0 1 0 0 256zM241 377c-9.4 9.4-24.6 9.4-33.9 0s-9.4-24.6 0-33.9l87-87-87-87c-9.4-9.4-9.4-24.6 0-33.9s24.6-9.4 33.9 0L345 239c9.4 9.4 9.4 24.6 0 33.9L241 377z" />
            </svg>
          </button>
        </div>
        <div class="first-hotgame-container">
          <a href="#" class="first-hotgame-link" :style="{ backgroundImage: `url(${getPath(`image/${themeColor}/lang/${lang}/first_hotgame0${n}.png`)})` }" v-for="n in 5" :key="n"></a>
        </div>
      </div>
    </section>
    <section class="first-section-wrap first-Platform-wrap">
      <div class="section-wrap">
        <a href="#" v-for="(platform, index) in ['game', 'lottery', 'card', 'live', 'sport']" :key="index">
          <img
            :src="platformHover === index
                ? getPath(`image/${themeColor}/lang/${lang}/first_platform_${platform}_h.png`)
                : getPath(`image/${themeColor}/lang/${lang}/first_platform_${platform}.png`)"
            @mouseenter="platformHover = index"
            @mouseleave="platformHover = null"
          />
        </a>
      </div>
    </section>
    <section class="first-section-wrap first-mobile-wrap">
      <div class="section-wrap">
        <div class="first-mobile-left-wrap">
          <div class="first-mobile-title"></div>
            <div class="first-intro-text">
              独家原生APP给玩家最优质的画面，便捷登录、操作简单，随时随地都可投注，极速稳定畅通，无阻多款游戏任由你畅玩。
            </div>
            <div class="first-mobile-info-wrap">
              <div class="first-mobile-qrcode-wrap">
                <div class="img-qrcode-bg">
                  <!-- <img src="/image/not-use/qrcode.jpg" alt=""> -->
                  <img :src="isLoggedIn
                    ? getPath(`image/not-use/qrcode.jpg`)
                    : getPath(`image/not-use/lang/${lang}/${imgQrcode}.png`)" class="first-mob-qrcode-img" />
                </div>
                <div class="first-mobile-info-title">扫码下载App</div>
                <p>支持 ios & Android</p>
                <div class="first-mobile-info-highlight">
                  <div class="first-mobile-info-icon ios"></div>
                  ios
                  <div class="first-mobile-info-icon android"></div>
                  Android
                </div>
              </div>
              <div class="first-mobile-h5-wrap">
                <div class="img-h5-bg"></div>
                <div class="first-mobile-info-title">直接访问</div>
                <p>手机输入网址即可访问</p>
                <div class="first-mobile-info-highlight">bbin.com</div>
              </div>
            </div>
        </div>
        <div class="first-mobile-right-wrap">
          <img v-for="index in 5" :key="index" :src="getPath(`image/first_mob0${index}.png`)"
            :class="`first-mobile-phone-img first-mobile-phone-img0${index}`" />
          <svg class="first-mobile-right-bg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="812px" height="722px"><path fill-rule="evenodd" fill="currentColor" d="M811.768,-0.000 L172.1000,-0.000 L-0.000,720.1000 L550.060,721.090 L701.564,86.965 C713.747,35.973 759.333,-0.000 811.768,-0.000 Z" /></svg>
        </div>
      </div>
    </section>
  </div>
</template>
<script setup>
import { storeToRefs } from 'pinia';
import News from '@/components/common/News.vue';
import HotGame from '@/components/common/HotGame.vue';
import FloatImg from '@/components/common/FloatImg.vue';
import { useTheme } from '@/composables/useTheme.js';
import { useAuthStore } from '@/stores/authStore.js';
import { getPath } from '@/composables/usePath.js'

const { themeColor, lang, imgQrcode } = useTheme(); // 使用动态主题和语言设定
const authStore = useAuthStore();
const { isLoggedIn } = storeToRefs(authStore); // 使用全域登入状态

import { Swiper, SwiperSlide } from 'swiper/vue';
import { Autoplay, Pagination } from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/pagination';
import 'swiper/css/autoplay';

// 轮播图资料
const slides = ref([
  { id: 1, image: 'slider01.jpg' },
  { id: 2, image: 'slider02.jpg' }
]);
const platformHover = ref(null);
</script>
