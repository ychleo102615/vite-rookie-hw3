<script setup>
import { ref, defineProps, defineEmits, watch } from 'vue'

const props = defineProps({
  id: { type: Number, required: true },
  name: { type: String, required: true },
  sub: { type: String, required: true },
  price: { type: Number, required: true },
  quantity: { type: Number, default: 1 },
  total: { type: Number, default: 0 },
})

// const quantity = ref(0)
const quantity = ref(props.quantity)

const emit = defineEmits(['update-quantity', 'remove'])

const updateQuantity = () => {
  // 發出事件通知父組件
  emit('update-quantity', props.id, quantity.value)
}

watch(
  () => props.quantity,
  (newQuantity) => {
    quantity.value = newQuantity
  },
)
watch(quantity, () => {
  emit('update-quantity', props.id, quantity.value)
})
</script>

<template>
  <!-- 這是一個table 中的 row-->
  <tr class="cart-item">
    <!-- x -->
    <td class="actions">
      <button @click="$emit('remove', props.id)">x</button>
    </td>
    <td class="name">{{ props.name }}</td>
    <td class="sub">{{ props.sub }}</td>
    <td class="quantity">
      <button @click="quantity -= 1" :disabled="quantity <= 1">-</button>
      {{ quantity }}
      <button @click="quantity += 1">+</button>
    </td>
    <td class="price">${{ props.price }}</td>
    <td class="total">${{ props.total }}</td>
  </tr>
</template>

<style scoped>
.cart-item {
  border-bottom: 1px solid #ccc;
}
td {
  padding: 8px;
}
</style>
