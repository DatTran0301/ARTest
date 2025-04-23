<template>
  <div
    v-if="showWarning"
    class="warning-box"
  >
    <p>{{ message }}</p>
    <button
      v-if="showChromeBtn"
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
const message = ref("")

const openInChrome = () => {
  const url = window.location.href
  const chromeUrl = "googlechrome://" + url.replace(/^https?:\/\//, "")
  window.location.href = chromeUrl
}

onMounted(() => {
  const ua = navigator.userAgent || navigator.vendor || window.opera
  const isIOS = /iPad|iPhone|iPod/.test(ua) && !window.MSStream
  const isNotChrome = !ua.includes("Chrome") || ua.includes("Edg") || ua.includes("OPR") || ua.includes("SamsungBrowser")
  const isInFacebookApp = ua.includes("FBAN") || ua.includes("FBAV")

  if ((isIOS || isNotChrome || isInFacebookApp)) {
    showWarning.value = true

    if (isIOS) {
      message.value = "Bạn đang dùng trình duyệt không tương thích trên iPhone. Vui lòng nhấn nút chia sẻ (↗️) và chọn 'Mở trong Safari' để trải nghiệm AR tốt hơn."
    } else {
      message.value = "Trình duyệt hiện tại không hỗ trợ tốt. Nhấn để mở bằng Chrome."
      showChromeBtn.value = true
    }
  }
})
</script>

<style scoped>
.warning-box {
  background-color: #fffbe6;
  border: 1px solid #ffe58f;
  border-radius: 8px;
  padding: 16px;
  margin: 16px;
}

.warning-box button {
  margin-top: 8px;
  padding: 8px 16px;
  background-color: #4285F4;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
