<template>
  <div class="member-wrap">
    <div class="login-wrap" v-if="!isLoggedIn">
      <form @submit.prevent="handleLogin">
        <div class="login-left">
          <label class="ele-login-unit ele-login-unit-account">
            <input
              id="js-username"
              title="请输入4-15个字元, 仅可输入英文字母以及数字的组合!!"
              name="username"
              type="text"
              maxlength="15"
              pattern="[a-zA-Z0-9]{4,15}"
              required="required"
              class="ele-login-input"
              v-model="username"
              @focus="handleFocus('username')"
              @blur="handleBlur('username')"
              @input="handleInput('username')"
            />
            <p class="ele-login-placeholder" :style="{ opacity: usernamePlaceholderOpacity }">账号</p>
            <span class="ele-acc-unicod">@bin</span>
          </label>
          <label class="ele-login-unit ele-login-unit-password">
            <input
              title="请填写 6-12 位大小写英数字"
              :type="passwordType"
              name="password"
              autocomplete="off"
              maxlength="12"
              pattern="[a-zA-Z0-9]{6,12}"
              required="required"
              class="ele-login-input"
              v-model="password"
              @focus="handleFocus('password')"
              @blur="handleBlur('password')"
              @input="handleInput('password')"
            />
            <p class="ele-login-placeholder" :style="{ opacity: passwordPlaceholderOpacity }">密码</p>
            <a href="#" :class="['ele-show-password', { 'is-show': isPasswordVisible }]" @click.prevent="togglePasswordVisibility">
            </a>
            <a href="#" title="忘记?" class="ele-forget-pw">忘记?</a>
          </label>
          <button type="submit" class="ele-login-submit">登入</button>
          <router-link to="/join" class="ele-join-btn">注册</router-link>
        </div>
      </form>
    </div>
    <div class="meminfo-wrap" v-else-if="isLoggedIn">
      <div class="meminfo-link-wrap">
        <div class="ele-meminfo-link-wrap">
          <ul>
            <template v-for="(link, index) in filteredLinks" :key="index">
              <li class="ele-meminfo-link">
                <a :href="link.link">{{ link.title }}</a>
              </li>
              <span v-if="showSeparator && (showLastSeparator ? index < (filteredLinks?.length ?? 0) : index < ((filteredLinks?.length ?? 0) - 1))" class="ele-meminfo-link-line">{{ separatorContent }}</span>
            </template>
          </ul>
        </div>
      </div>
      <div class="mem-info-wrap">
        <div class="ele-meminfo-wrap">
          <div class="ele-meminfo ele-meminfo-name">
            <span>账号：</span>
            <strong>{{ user?.fullUsername || 'admin' }}</strong>
          </div>
          <div class="ele-meminfo-unit">
            <div class="ele-meminfo ele-meminfo-balance">
              <span>BBIN余额：</span>
              <strong>168.08</strong>
            </div>
            <div class="ele-accinfo ele-other-balance" @mouseenter="showBalance = true" @mouseleave="showBalance = false">
              <div class="ele-other-balance-icon"></div>
            </div>
            <transition name="fade">
              <div v-show="showBalance" @mouseenter="showBalance = true" @mouseleave="showBalance = false" class="ele-balance-wrap">
                <div class="ele-balance-item">
                  <div class="ele-balance">
                    <span>AG视讯余额：</span>
                    <strong>--</strong>
                  </div>
                  <div class="ele-balance">
                    <span>欧博视讯余额：</span>
                    <strong>68.00</strong>
                  </div>
                  <div class="ele-balance">
                    <span>BG视讯余额：</span>
                    <strong>100.00</strong>
                  </div>
                </div>
                <div class="ele-balance-tool">
                  <a href="#" class="ele-balance-btn">额度转换</a>
                </div>
              </div>
            </transition>
          </div>
        </div>
        <button type="button" class="ele-logout-btn" @click="handleLogout">登出</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { storeToRefs } from 'pinia';
import { useDataStore } from '@/stores/dataStore';
import { useAuthStore } from '@/stores/authStore';

const dataStore = useDataStore();
const authStore = useAuthStore();
const route = useRoute();

// 🔸 控制登入後會員連結分隔符號
const props = defineProps({
  showSeparator: {
    type: Boolean,
    default: true
  },
  separatorContent: {
    type: String,
    default: '|'
  },
  showLastSeparator: {
    type: Boolean,
    default: false // 控制最後一個項目是否顯示分隔符號
  }
});

// 使用 authStore 的登入狀態
const { isLoggedIn, user } = storeToRefs(authStore);

// 根據當前路由判斷 navClass，然後顯示對應的 meminfoLinks
const filteredLinks = computed(() => {
  // 從 headerNav 中找到當前路由對應的 navClass
  const currentNav = dataStore.headerNav.find(nav => nav.link === route.path);
  const navClass = currentNav?.navClass || 'else';

  // 根據 navClass 取得對應的 meminfoLinks，如果沒有則使用 'else'
  return dataStore.meminfoLinks[navClass] || dataStore.meminfoLinks['else'] || [];
});


// 登入欄位點擊後透明度效果
const username = ref('');
const password = ref('');
const isPasswordVisible = ref(false);

const usernamePlaceholderOpacity = ref(1);
const passwordPlaceholderOpacity = ref(1);

const handleFocus = (field) => {
  if (field === 'username' && username.value.trim() === '') {
    usernamePlaceholderOpacity.value = 0.5;
  }
  if (field === 'password' && password.value.trim() === '') {
    passwordPlaceholderOpacity.value = 0.5;
  }
};

const handleBlur = (field) => {
  if (field === 'username') {
    usernamePlaceholderOpacity.value = username.value.trim() === '' ? 1 : 0;
  }
  if (field === 'password') {
    passwordPlaceholderOpacity.value = password.value.trim() === '' ? 1 : 0;
  }
};

const handleInput = (field) => {
  if (field === 'username') {
    usernamePlaceholderOpacity.value = username.value.trim() !== '' ? 0 : 0.5;
  }
  if (field === 'password') {
    passwordPlaceholderOpacity.value = password.value.trim() !== '' ? 0 : 0.5;
  }
};

const togglePasswordVisibility = () => {
  isPasswordVisible.value = !isPasswordVisible.value;
};

const passwordType = computed(() => (isPasswordVisible.value ? 'text' : 'password'));

// 登入處理函數
const handleLogin = () => {
  if (!username.value.trim() || !password.value.trim()) {
    alert('請輸入帳號和密碼');
    return;
  }

  const success = authStore.login({
    username: username.value.trim(),
    password: password.value.trim()
  });

  if (success) {
    // 登入成功後清除表單
    username.value = '';
    password.value = '';
    usernamePlaceholderOpacity.value = 1;
    passwordPlaceholderOpacity.value = 1;
    isPasswordVisible.value = false;

    console.log('登入成功:', authStore.user?.username);
  } else {
    alert('請輸入有效的帳號和密碼');
  }
};

// 登出處理函數
const handleLogout = () => {
  authStore.logout();

  // 重置表單狀態
  username.value = '';
  password.value = '';
  usernamePlaceholderOpacity.value = 1;
  passwordPlaceholderOpacity.value = 1;
  isPasswordVisible.value = false;

  console.log('已登出');
};

// 控制餘額顯示
const showBalance = ref(false);
</script>
