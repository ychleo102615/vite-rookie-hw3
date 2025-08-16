<script setup>
import CartItem from './components/CartItem.vue'
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
</script>

<template>
  <main>
    <section class="container">
      <!--  左側餐點項目 -->
      <section class="left">
        <h2>餐點項目</h2>
        <section
          class="product-card"
          v-for="(item, index) in itemList"
          :key="index"
          @click="addToCart(item)"
        >
          <div class="card-header">
            <h2 class="name" style="line-height: 1.2; margin: 0">{{ item.name }}</h2>
            <span class="price">${{ item.price }}</span>
          </div>
          <p class="description" style="line-height: 1.4; margin: 0">{{ item.sub }}</p>
        </section>
      </section>

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
            <!-- <th>操作</th>
            <th>品項</th>
            <th>描述</th>
            <th>數量</th>
            <th>單價</th>
            <th>小記</th> -->
          </thead>
          <tbody>
            <CartItem
              v-for="item in cartList"
              :key="item.id"
              :id="item.id"
              :name="item.name"
              :sub="item.sub"
              :price="item.price"
              :quantity="item.quantity"
              :total="item.total"
              @remove="remove"
              @update-quantity="updateQuantity"
            />
          </tbody>
        </table>
        <div class="please-select" v-if="cartList.length === 0">
          <p>請選擇商品</p>
        </div>
      </section>
    </section>
  </main>
  <footer>
    <!-- 訂單  -->
  </footer>
</template>

<style scoped>
main {
  display: flex;
  justify-content: center;
}

.container {
  display: flex;
}

.left {
  width: 300px; /* 左側固定寬度 */
  /* background: #f0f0f0; */
}
.right {
  width: 600px; /* 右側固定寬度 */
  background: #f9f9f9;
  margin-left: 20px; /* 右側與左側之間的間距 */
}

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

/* p {
  margin: 0;
  line-height: 1.4;
} */

/* h2 {
  margin: 0 0 8px;
  line-height: 1.2;
} */

table {
  border-collapse: collapse; /* 或者用 border-spacing: 0; */
  /* border-spacing: 0;        // 若不想用 collapse 也可改這行 */
  width: 100%;
}

thead th {
  border-bottom: 2px solid #000; /* 粗黑線 */
}
</style>
