<template>
  <div class="home">
    <div class="content">
      <h1 v-if="active === 'open'" class="content-title">开仓</h1>
      <TradeForm v-if="active === 'open'" />
      <div v-if="active === 'add'" class="content-item">添加记录</div>
      <div v-if="active === 'history'" class="content-item">历史记录</div>
    </div>
    <van-tabbar v-model="active" class="safe-area-bottom">
      <van-tabbar-item name="open" icon="plus">开仓</van-tabbar-item>
      <van-tabbar-item name="add" icon="add-o">添加记录</van-tabbar-item>
      <van-tabbar-item name="history" icon="records">历史记录</van-tabbar-item>
    </van-tabbar>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import TradeForm from './TradeForm.vue'

export default defineComponent({
  name: 'Home',
  components: {
    TradeForm
  },
  setup() {
    const active = ref('open')

    return {
      active
    }
  }
})
</script>

<style scoped>
.home {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100vw;
  background-color: #eff2f5;
}

.content {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
}

.content-title {
  margin-bottom: 20px;
}

.content-item {
  text-align: center;
  padding-top: 50px;
  font-size: 18px;
}

.safe-area-bottom {
  padding-bottom: env(safe-area-inset-bottom);
}
</style>

