<script setup>
import { ref, defineProps } from 'vue'

const props = defineProps({
  orderedItems: {
    type: Array,
    default: () => [],
  },
  total: {
    type: Number,
    default: 0,
  },
  orderMemo: {
    type: String,
    default: '',
  },
})
</script>

<template>
  <div class="container">
    <p class="title">訂單</p>
    <table>
      <colgroup>
        <col style="width: 55%" />
        <col style="width: 20%" />
        <col style="width: 25%" />
      </colgroup>
      <thead>
        <tr>
          <th>品項</th>
          <th>數量</th>
          <th>小計</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in props.orderedItems" :key="item.id">
          <td>{{ item.name }}</td>
          <td>{{ item.quantity }}</td>
          <td>${{ item.total }}</td>
        </tr>
      </tbody>
    </table>
    <p class="memo">備註: {{ props.orderMemo }}</p>
    <p class="total">總計: ${{ props.total }}</p>
  </div>
</template>

<style>
.title {
  font-size: 1.5rem;
  margin: 0;
}

.memo {
  text-align: right;
}

.total {
  text-align: right;
  font-weight: bold;
  font-size: 1.2rem;
  margin: 0;
}

table {
  width: 100%;
  margin: 10px;
  text-align: left;
  border-collapse: collapse;
}

th,
td {
  border-bottom: 1px solid #bbbbbb;
}
</style>
