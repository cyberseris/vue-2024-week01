<template>
  <div class="container d-flex flex-column">
    <table class="wrapper border border-radius pa-20">
      <thead>
        <tr class="text-center fz-20">
          <th scope="col" class="fw-700">品項</th>
          <th scope="col" class="fw-700">描述</th>
          <th scope="col" class="fw-700">價格</th>
          <th scope="col" class="fw-700">數量</th>
          <th scope="col" class="fw-700">操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in items" :key="item.id" class="text-center">
          <td v-if="!item.isEditable" class="fz-16" style="height: 50px !important">
            {{ item.name }}
          </td>
          <td>
            <small class="fz-16">{{ item.description }}</small>
          </td>
          <td class="fz-16">{{ item.price }}</td>
          <td class="fz-16">
            <button type="button" @click="minusItem(item)" class="mr-8 fz-16">-</button>
            {{ item.num }}
            <button type="button" @click="addItem(item)" class="ml-8 fz-16">+</button>
          </td>
          <td>
            <button type="button" @click="editItem(item)" class="fz-16">編輯</button>
          </td>
        </tr>
        <tr class="text-right">
          <td colspan="5" class="fz-20 h-100">
            <span class="mr-60 fw-700">小計： {{ price }}</span>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="isEditable" class="mt-50 wrapper d-flex justify-end">
      <span class="mr-16 fz-16"
        >修改品項為：<input type="text" v-model="tempItem" class="fz-16"
      /></span>
      <button type="button" @click="confirmEdit()" class="mr-16 fz-16">確定編輯</button>
      <button type="button" @click="cancelEdit()" class="fz-16">取消編輯</button>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, watch } from 'vue'

let isEditable = ref(false)
let tempId = ref(0)
let tempItem = ref('')
let price = ref(0)
let items = reactive([
  {
    id: 1,
    name: '珍珠奶茶',
    description: '香濃奶茶搭配 QQ 珍珠',
    price: 50,
    num: 0,
    isEditable: false
  },
  {
    id: 2,
    name: '冬瓜檸檬',
    description: '清新冬瓜配上新鮮檸檬',
    price: 45,
    num: 0,
    isEditable: false
  },
  {
    id: 3,
    name: '翡翠檸檬',
    description: '綠茶與檸檬的完美結合',
    price: 55,
    num: 0,
    isEditable: false
  },
  {
    id: 4,
    name: '四季春茶',
    description: '香醇四季春茶，回甘無比',
    price: 45,
    num: 0,
    isEditable: false
  },
  {
    id: 5,
    name: '阿薩姆奶茶',
    description: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50,
    num: 0,
    isEditable: false
  },
  {
    id: 6,
    name: '檸檬冰茶',
    description: '檸檬與冰茶的清新組合',
    price: 45,
    num: 0,
    isEditable: false
  },
  {
    id: 7,
    name: '芒果綠茶',
    description: '芒果與綠茶的獨特風味',
    price: 55,
    num: 0,
    isEditable: false
  },
  {
    id: 8,
    name: '抹茶拿鐵',
    description: '抹茶與鮮奶的絕配',
    price: 60,
    num: 0,
    isEditable: false
  }
])

watch(
  items,
  (newItems) => {
    price.value = newItems.reduce((sum, item) => sum + item.price * item.num, 0)
  },
  { deep: true }
)

const addItem = (item) => {
  item.num++
}
const minusItem = (item) => {
  if (item.num === 0) {
    item.num = 0
  } else {
    item.num--
  }
}

const editItem = (item) => {
  isEditable.value = !isEditable.value
  tempId.value = item.id
  tempItem.value = item.name
}

const confirmEdit = () => {
  items.filter((item) => {
    if (item.id == tempId.value) {
      item.name = tempItem.value
    }
  })

  isEditable.value = !isEditable.value
}

const cancelEdit = () => {
  isEditable.value = !isEditable.value
}
</script>

<style>
.container {
  width: 1080px;
  height: 600px;
  margin: 0 auto;
}
.wrapper {
  width: 800px;
  height: 300px;
  margin: 0 auto;
  margin-top: 16px;
}
.d-flex {
  display: flex;
}
.flex-column {
  flex-direction: column;
}
.mt-50 {
  margin-top: 50px;
}
.ml-8 {
  margin-left: 8px;
}
.mr-8 {
  margin-right: 8px;
}
.mr-16 {
  margin-right: 16px;
}
.mr-60 {
  margin-right: 60px;
}
.h-80 {
  height: 80px;
}
.fz-16 {
  font-size: 16px;
}
.fz-20 {
  font-size: 20px;
}
.fw-700 {
  font-weight: 700;
}
.justify-end {
  justify-content: end;
}
.text-center {
  text-align: center;
}
.text-right {
  text-align: right;
}
.pa-20 {
  padding: 20px;
}
.border {
  border: 1px solid #ccc;
}
.border-radius {
  border-radius: 30px;
}
</style>
