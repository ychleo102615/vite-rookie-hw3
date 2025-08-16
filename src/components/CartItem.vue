<script setup>
import { ref, defineProps, defineEmits, watch } from 'vue'

const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
})

// const quantity = ref(0)
const quantity = ref(props.item.quantity)

const emit = defineEmits(['update-quantity', 'remove'])

function onQtyChange() {
  const qty = Math.max(1, Math.floor(Number(quantity.value) || 1))
  emit('update-quantity', { id: props.item.id, qty })
}

watch(
  () => props.item.quantity,
  (newQuantity) => {
    quantity.value = newQuantity
  },
)
watch(quantity, () => {
  emit('update-quantity', props.item.id, quantity.value)
})
</script>

<template>
  <tr class="cart-item">
    <td class="actions">
      <button @click="emit('remove', item.id)">x</button>
    </td>
    <td class="name">{{ item.name }} xxx</td>
    <td class="sub">{{ item.sub }}</td>
    <td>
      <input type="number" min="1" step="1" v-model="quantity" @input="onQtyChange" />
    </td>
    <td class="price">${{ item.price }}</td>
    <td class="total">${{ item.total }}</td>
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
