<template>
  <div class="page-container">
    <section class="first-section-wrap first-game-wrap">
      <div class="section-wrap">
        <div class="first-game-tabs">
          <button
            v-for="(tab, key) in firstGame"
            :key="key"
            class="tab"
            :class="{ active: activeTab === key }"
            @click="activeTab = key"
          >
            <span>{{ tab.label }}</span>
          </button>
        </div>
        <transition name="fade" mode="out-in">
          <div class="first-game-box" v-if="firstGame[activeTab]?.game?.length">
            <div class="first-game-items">
              <a href="#" class="first-game-item" v-for="(game, index) in firstGame[activeTab].game" :key="index">
                <div class="item-img">
                  <button class="item-btn"><i></i></button>
                  <img :src="getPath(`image/${themeColor}/first_game_${activeTab}${String(index + 1).padStart(2, '0')}.png`)" />
                </div>
                <div class="item-name">
                  <div class="name">{{ game }}</div>
                </div>
              </a>
            </div>
            <div :class="`first-game-title first-game-title-${activeTab}`">
              <img :src="getPath(`image/${themeColor}/first_game_${activeTab}_title.png`)" class="first-game-title-img" />
            </div>
          </div>
        </transition>
      </div>
    </section>
    <section class="first-section-wrap first-mob-wrap" :style="{ backgroundImage: `url(${getPath(`image/${themeColor}/first_mob_bg.png`)})` }">
      <div class="section-wrap">
        <div class="first-mob-left-wrap">
          <img class="first-mob-phone" :src="getPath(`image/bg_mobile.png`)">
        </div>
        <div class="first-mob-right-wrap">
          <div class="first-mob-title">
            业内顶尖原生APP
          </div>
          <div class="first-mob-description">
            最顶尖的手机投注APP希望您始终能感受到品牌对<br>
            专业的执着！原生体育APP，便捷登录、操作简单、<br><br></br>
            界面一目了然、游戏畅通无阻、酷炫玩法全覆盖，
            指尖体育APP精彩无处不在。
          </div>
          <div class="first-mob-download">
            <div class="first-mob-qrcode">
              <div class="first-mob-bg">
                <img :src="isLoggedIn
                ? getPath('image/not-use/qrcode.jpg')
                : getPath(`image/not-use/lang/${lang}/${imgQrcode}.png`)" class="first-mob-qrcode-img" />
              </div>
              <div class="first-mob-text"><div class="first-mob-text-top">扫码下载APP</div><div class="first-mob-text-bottom">iOS & Android</div></div>
            </div>
            <div class="first-mob-h5">
              <div class="first-mob-bg">
                <div class="first-mob-h5-img"></div>
              </div>
              <div class="first-mob-text"><div class="first-mob-text-top">无需下载直接访问</div><div class="first-mob-text-bottom">web.bbinpartner.com</div></div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script setup>
import { storeToRefs } from 'pinia';
import { useTheme } from '@/composables/useTheme.js';
import { useAuthStore } from '@/stores/authStore.js';
import { getPath } from '@/composables/usePath.js'
import FloatImg from '@/components/common/FloatImg.vue';

const { themeColor, lang, imgQrcode } = useTheme(); // 使用动态主题和语言设定
const authStore = useAuthStore();
const { isLoggedIn } = storeToRefs(authStore); // 使用全域登入状态
const enableThemeManager = inject('enableThemeManager')

const activeTab = ref('sports');
const firstGame = ref({
  sports: {
    label: '体育赛事',
    game: ['BB体育', '皇冠体育', '波音体育', '沙巴体育', 'New BB体育', '沙巴电竞', 'New BB电竞', '熊猫体育']
  },
  live: {
    label: '视讯直播',
    game: ['BB视讯', 'PA视讯', 'BG视讯', 'MG视讯', 'PT视讯', 'EVO视讯', 'DB视讯', 'PP视讯']
  },
  casino: {
    label: '电子游艺',
    game: ['BB电子', 'JDB电子', 'MG电子', 'PA电子', 'CQ9电子', '大满贯电子', 'PG电子', 'PT电子']
  },
  lottery: {
    label: '彩票游戏',
    game: ['BB彩票', 'VR彩票', 'CC彩票']
  },
  card: {
    label: '棋牌游戏',
    game: ['BB棋牌', '开元棋牌', 'JDB棋牌', 'FG棋牌', 'WG棋牌', 'MT棋牌', '乐游棋牌', '百胜棋牌']
  },
  fish: {
    label: '捕鱼游戏',
    game: ['BB捕鱼', 'CQ9捕鱼', 'MT捕鱼', 'PA捕鱼', 'JDB捕鱼', 'PP捕鱼', 'VA捕鱼', 'FG捕鱼']
  }
});
</script>