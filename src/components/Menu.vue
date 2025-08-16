<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  itemList: {
    type: Array,
    default: () => [],
  },
})

const emit = defineEmits(['add-to-cart'])
</script>

<template>
  <section>
    <h2>餐點項目</h2>
    <section
      class="product-card"
      v-for="(item, index) in props.itemList"
      :key="index"
      @click="emit('add-to-cart', item)"
    >
      <div class="card-header">
        <h2 class="name" style="line-height: 1.2; margin: 0">{{ item.name }}</h2>
        <span class="price">${{ item.price }}</span>
      </div>
      <p class="description" style="line-height: 1.4; margin: 0">{{ item.sub }}</p>
    </section>
  </section>
</template>

<style scoped>
/* 卡片樣式 可點擊 hover變色 */
.product-card {
  box-sizing: border-box;
  border: 1px solid #ccc;
  padding: 12px;
  width: 100%;
  margin-bottom: -1px;
  cursor: pointer;
}

.product-card:hover {
  background: #f9f9f9; /* 滑鼠移入時背景變色 */
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
}

.name {
  font-size: 1.2em;
  font-weight: bold;
}

.price {
  font-weight: bold;
}

.description {
  font-size: 0.9em;
  color: #555;
}
</style>
