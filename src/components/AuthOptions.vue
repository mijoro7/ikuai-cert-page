<template>
  <div class="auth-options">
    <div class="choose-text">
      <p class="choose-en">Network connection</p>
    </div>
    <div class="auth-list">
      <div v-for="method in authMethods" :key="method.type" class="auth-button" :style="{ background: method.gradient, boxShadow: method.boxShadow }" @click="doAuth(method.type)">
        <div class="icon-container">
          <img :src="method.icon" :alt="method.label" class="auth-icon" />
        </div>
        <div class="label-container">
          <span class="auth-label">{{ method.label }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';
import { getParameterByName, IsPC } from '../utils/helpers';

const props = defineProps<{
  authMethods: { type: number; label: string; icon: string; gradient: string; boxShadow: string }[];
}>();

const doAuth = (type: number) => {
  const sUserAgent = navigator.userAgent.toLowerCase();
  let DstAddr = "/Action/webauth-up?";
  const Search = location.search.replace('?', "&");

  switch (type) {
    case 17:
      const ip = getParameterByName('user_ip');
      if (ip) {
          const encodedIp = btoa(ip);
          DstAddr = `http://yy.ikuai8.net/RedPacket/index?ip=${encodedIp}&mac=${getParameterByName('mac')}&gwid=${getParameterByName('gwid')}`;
      }

      if (sUserAgent.match(/micromessenger/i)) {
        DstAddr = `https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx03c25ea1a4e4f7ac&redirect_uri=${encodeURIComponent(DstAddr)}&response_type=code&scope=snsapi_base&state=${getParameterByName('mac')}#wechat_redirect`;
      }
      break;
    case 19:
      const roomNum = getParameterByName('roomNum');
      const cardId = getParameterByName('cardId');
      DstAddr = `/Action/webauth-up${location.search}&roomNum=${roomNum}&cardId=${cardId}&type=19`;
      break;
    default:
      let modifiedSearch = Search;
      if (getParameterByName('sm') === 'yes') {
        modifiedSearch = modifiedSearch.replace(/\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}/, "");
      } else if (sUserAgent.match(/micromessenger/i)) {
        modifiedSearch = "&sm=yes" + modifiedSearch;
        modifiedSearch = modifiedSearch.replace(/\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}/, "");
      }

      if (type === 12 && !sUserAgent.match(/micromessenger/i)) {
        if (IsPC() && "0" === "1") { // Assuming wechat_noauth is a global variable, we check it directly.
          location.href = "/Action/wx_release?extend=" + getParameterByName("user_ip") + '_1';
          return;
        } else {
          alert('Please open this page in WeChat on a mobile device!');
          return;
        }
      }

      const Type = "type=" + type;
      DstAddr = DstAddr + Type + modifiedSearch;
  }
  location.href = DstAddr;
};

</script>

<style scoped>
.auth-options {
  width: 100%;
  margin-top: 1rem;
}

.choose-text {
  text-align: center;
  margin-bottom: 1rem;
}

.choose-en {
  color: #999;
  font-size: 0.9rem;
}

.auth-list {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.auth-button {
  display: flex;
  align-items: center;
  padding: 0.8rem 1rem;
  border-radius: 2rem;
  color: white;
  cursor: pointer;
  transition: transform 0.2s ease-in-out;
  width: 100%;
  box-sizing: border-box;
  max-width: 400px;
  margin: 0 auto;
}

.auth-button:hover {
  transform: translateY(-3px);
}

.icon-container {
  width: 2.5rem;
  height: 2.5rem;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.2);
  display: flex;
  justify-content: center;
  align-items: center;
  margin-right: 1rem;
}

.auth-icon {
  width: 1.8rem;
  height: 1.8rem;
}

.label-container {
  flex-grow: 1;
  text-align: left;
}

.auth-label {
  font-size: 1rem;
}
</style>
