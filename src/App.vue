<script setup>
import CartItem from './components/CartItem.vue'
import Order from './components/Order.vue'
import Menu from './components/Menu.vue'
import { ref } from 'vue'

const data = [
  { id: 1, name: '珍珠奶茶', sub: '香濃奶茶搭配', price: 50 },
  { id: 2, name: '冬瓜檸檬', sub: '清新冬瓜配上新鮮檸檬', price: 45 },
  { id: 3, name: '翡翠檸檬', sub: '綠茶與檸檬的完美結合', price: 55 },
  { id: 4, name: '四季春茶', sub: '香醇四季春茶', price: 45 },
  { id: 5, name: '阿薩姆奶茶', sub: '阿薩姆紅茶搭配香醇鮮奶', price: 50 },
  { id: 6, name: '檸檬冰茶', sub: '檸檬與冰茶的清新組合', price: 45 },
  { id: 7, name: '芒果綠茶', sub: '芒果與綠茶的獨特風味', price: 55 },
  { id: 8, name: '抹茶拿鐵', sub: '抹茶與鮮奶的絕配', price: 60 },
]
const titles = ['操作', '品項', '描述', '數量', '單價', '小記']

const itemList = ref(data)

const cartList = ref([])

const addToCart = (item) => {
  const existingItem = cartList.value.find((cartItem) => cartItem.id === item.id)
  if (existingItem) {
    existingItem.quantity++
    existingItem.total = existingItem.quantity * existingItem.price
  } else {
    cartList.value.push({
      ...item,
      quantity: 1,
      total: item.price,
    })
  }
}

const updateQuantity = (id, quantity) => {
  const cartItem = cartList.value.find((item) => item.id === id)
  if (cartItem) {
    cartItem.quantity = quantity
    cartItem.total = cartItem.quantity * cartItem.price
  } else {
    console.log('no item found with id:', id)
    console.log(cartList.value)
  }
}

const remove = (id) => {
  cartList.value = cartList.value.filter((item) => item.id !== id)
}

const orderedItems = ref([])
const orderingMemo = ref('')
const orderedMemo = ref('')
const maxLength = 100
const orderedTotal = ref(0)

const sendOrder = () => {
  if (cartList.value.length === 0) {
    alert('請先選擇商品')
    return
  }
  orderedItems.value = [...cartList.value]
  cartList.value = []
  orderedMemo.value = orderingMemo.value
  orderingMemo.value = ''
  orderedTotal.value = orderedItems.value.reduce((sum, item) => sum + item.total, 0)
}
</script>

<template>
  <main>
    <div class="container">
      <section class="ordering-container">
        <!--  左側餐點項目 -->
        <Menu class="left" :item-list="itemList" @add-to-cart="addToCart"></Menu>

        <!--  右側點餐畫面 -->
        <section class="right">
          <h2>點餐畫面</h2>
          <table>
            <colgroup>
              <col style="width: 10%" />
              <col style="width: 15%" />
              <col style="width: 35%" />
              <col style="width: 15%" />
              <col style="width: 10%" />
              <col style="width: 15%" />
            </colgroup>
            <thead>
              <tr>
                <th v-for="title in titles" :key="title">
                  {{ title }}
                </th>
              </tr>
            </thead>
            <tbody>
              <CartItem
                v-for="item in cartList"
                :key="item.id"
                :item="item"
                @remove="remove"
                @update-quantity="updateQuantity"
              />
            </tbody>
          </table>
          <div class="please-select" v-if="cartList.length === 0">
            <p>請選擇商品</p>
          </div>
          <div v-else>
            <h3 style="line-height: 1.2; text-align: right">
              總計: ${{ cartList.reduce((sum, item) => sum + item.total, 0) }}
            </h3>
            <!-- 備注區塊 -->
            <textarea
              v-model.trim="orderingMemo"
              :maxlength="maxLength"
              class="memo-input"
              rows="3"
              placeholder="備註"
            ></textarea>
            <button class="send-order" @click="sendOrder">送出</button>
          </div>
        </section>
      </section>
      <Order
        v-if="orderedItems.length > 0"
        class="order-com"
        :ordered-items="orderedItems"
        :total="orderedTotal"
        :order-memo="orderedMemo"
      />
      <div v-else class="not-ordered order-com">尚未建立訂單</div>
    </div>
  </main>
</template>

<style scoped>
main {
  display: flex;
  justify-content: center;
}

/* .container {
  display: flex;
} */

.ordering-container {
  display: flex;
}

.left {
  width: 300px; /* 左側固定寬度 */
  /* background: #f0f0f0; */
}
.right {
  width: 600px;
  /* background: #f9f9f9; */
  margin-left: 20px;
}

.please-select {
  display: flex;
  justify-content: center;
  align-items: center;
  /* height: 85px; */
  background: #e6f2ff; /* 淺藍背景 */
  border: 1px solid #3399ff; /* 稍深藍色粗框 */
  border-radius: 8px; /* 圓角 */
  color: #003366; /* 深藍文字 */
  font-weight: bold;
  margin-top: 8px;
}

.memo-input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.send-order {
  background-color: #2563eb; /* 藍底 */
  color: #fff; /* 白字 */
  border: none;
  border-radius: 6px; /* 圓角 */
  padding: 8px 16px;
  cursor: pointer;
  float: right; /* 向右靠齊 */
  margin-top: 8px;
}
.send-order:hover {
  background-color: #1e4db7; /* hover 稍深藍 */
}

.not-ordered {
  text-align: center;
  background-color: #e6e6e6;
}

.order-com {
  width: 70%;
  /* justify-self: center; */
  align-self: center;
  margin: 20px auto 0; /* 置中：左右 auto */
  padding: 16px;
  border-radius: 6px;
  border: 0.1px solid #adadad;
}

table {
  border-collapse: collapse; /* 或者用 border-spacing: 0; */
  /* border-spacing: 0;        // 若不想用 collapse 也可改這行 */
  width: 100%;
}

thead th {
  border-bottom: 2px solid #000; /* 粗黑線 */
}
</style>
