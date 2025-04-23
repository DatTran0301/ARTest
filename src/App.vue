<template>
  <div
    v-if="showWarning || showChromeOk"
    class="warning-box"
  >
    <p class="message">{{ message }}</p>

    <button
      v-if="showChromeBtn"
      class="action-btn"
      @click="openInChrome"
    >
      Mở bằng Chrome
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const showWarning = ref(false)
const showChromeBtn = ref(false)
const showChromeOk = ref(false)
const message = ref("")

const openInChrome = () => {
  const url = window.location.href
  const chromeUrl = "googlechrome://" + url.replace(/^https?:\/\//, "")
  window.location.href = chromeUrl
}

onMounted(() => {
  const ua = navigator.userAgent || navigator.vendor || window.opera
  const isIOS = /iPad|iPhone|iPod/.test(ua) && !window.MSStream
  const isInFacebookApp = ua.includes("FBAN") || ua.includes("FBAV")
  const isChrome = ua.includes("Chrome") && !ua.includes("Edg") && !ua.includes("OPR") && !ua.includes("SamsungBrowser") && !ua.includes("Zalo")

  if (isChrome && !isInFacebookApp) {
    showChromeOk.value = true
    message.value = "✅ Bạn đang dùng trình duyệt tương thích – Chrome."
    return
  }

  showWarning.value = true

  if (isIOS) {
    message.value = "⚠️ Bạn đang dùng trình duyệt không tương thích trên iPhone. Vui lòng nhấn nút chia sẻ (↗️) rồi chọn 'Mở trong Safari' để trải nghiệm AR tốt hơn."
  } else {
    message.value = "⚠️ Trình duyệt hiện tại không hỗ trợ tốt. Nhấn nút bên dưới để mở bằng Chrome."
    showChromeBtn.value = true
  }
})
</script>

<style scoped>
.warning-box {
  background-color: #fffbe6;
  border: 1px solid #ffe58f;
  border-radius: 12px;
  padding: 16px;
  margin: 16px auto;
  max-width: 480px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.message {
  font-size: 1rem;
  line-height: 1.5;
  color: #664d03;
}

.action-btn {
  margin-top: 12px;
  padding: 10px 20px;
  background-color: #4285F4;
  color: white;
  font-size: 1rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  width: 100%;
  max-width: 240px;
}
</style>
