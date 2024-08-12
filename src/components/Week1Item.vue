<script setup>
  import { ref } from 'vue'

  const drinkList = ref([
    {
      id: generateId(),
      title: "珍珠奶茶",
      describe: "香濃奶茶搭配QQ珍珠",
      price: 50,
      inventory: 20,
      isDeleted: false
    },
    {
      id: generateId(),
      title: "冬瓜檸檬",
      describe: "清新冬瓜配上新鮮檸檬",
      price: 45,
      inventory: 18,
      isDeleted: false
    },
    {
      id: generateId(),
      title: "翡翠檸檬",
      describe: "綠茶與檸檬的完美結合",
      price: 55,
      inventory: 34,
      isDeleted: false
    },
    {
      id: generateId(),
      title: "四季春茶",
      describe: "香醇四季春茶，回甘無比",
      price: 45,
      inventory: 10,
      isDeleted: false
    },
    {
      id: generateId(),
      title: "阿薩姆奶茶",
      describe: "阿薩姆紅茶搭配香醇鮮奶",
      price: 50,
      inventory: 25,
      isDeleted: false
    },
    {
      id: generateId(),
      title: "檸檬冰茶",
      describe: "檸檬與冰茶的清新組合",
      price: 45,
      inventory: 20,
      isDeleted: false
    },
    {
      id: generateId(),
      title: "芒果綠茶",
      describe: "芒果與綠茶的獨特風味",
      price: 55,
      inventory: 18,
      isDeleted: false
    },
    {
      id: generateId(),
      title: "抹茶拿鐵",
      describe: "抹茶與鮮奶的絕配",
      price: 60,
      inventory: 20,
      isDeleted: false
    }
  ])
  const action = ref('')
  const tempAddItem = ref({
    id: generateId(),
    title: "",
    describe: "",
    price: 0,
    inventory: 0,
    isDeleted: false
  })
  const isEditing = ref(false)
  const isDeleting = ref(false)
  const tempEditList = ref([])
  const tempDeleteList = ref([])

  // 產亂數
  function generateId() {
    return Date.now().toString(36) + Math.random().toString(36).substring(2, 11)
  }

  function decreaseInventory(item) {
    item.inventory -= 1
  }

  function increaseInventory(item) {
    item.inventory += 1
  } 

  function addItem() {
    action.value = 'add'
  }

  function editItem() {
    action.value = 'edit'
    tempEditList.value = drinkList.value.map(item => ({ ...item }))
    isEditing.value = !isEditing.value
  }

  function deleteList() {
    action.value = 'delete'
    isDeleting.value = !isDeleting.value
  }

  function deleteItem(item) {
    tempDeleteList.value.push(item.id)
    item.isDeleted = true
    console.log(item)
  }

  function cancelDeleteItem(item) {
    const index = tempDeleteList.value.findIndex(v => v === item.id)
    if (index !== -1) {
      tempDeleteList.value.splice(index, 1)
      item.isDeleted = false
    }
  }

  function saveList() {
    if (action.value === 'add') {
      drinkList.value.unshift(tempAddItem.value)
      tempAddItem.value = {
        id: generateId(),
        title: "",
        describe: "",
        price: 0,
        inventory: 0,
        isDeleted: false
      }
    }

    if (action.value === 'delete') {
      drinkList.value = drinkList.value.filter(v => tempDeleteList.value.indexOf(v.id) < 0)
      isDeleting.value = !isDeleting.value
    }

    if (action.value === 'edit') {
      isEditing.value = !isEditing.value
    }

    action.value = ''
  }

  function cancelList() {
    if (action.value === 'add') {
      tempAddItem.value = {
        id: generateId(),
        title: "",
        describe: "",
        price: 0,
        inventory: 0,
        isDeleted: false
      }
    }

    if (action.value === 'delete') {
      isDeleting.value = !isDeleting.value
      drinkList.value.forEach(item => {
        if (tempDeleteList.value.includes(item.id)) item.isDeleted = false
      })
    }

    if (action.value === 'edit') {
      isEditing.value = !isEditing.value
      drinkList.value = tempEditList.value
      tempEditList.value = []
    }

    action.value = ''
  }
  
</script>

<template>
  <div>
    <div v-if="action === ''">
      <button type="button" @click="addItem">新增品項</button>
      <button type="button" @click="editItem">編輯品項</button>
      <button type="button" @click="deleteList">刪除品項</button>
    </div>
    <div v-else>
      <button type="button" @click="saveList">保存品項</button>
      <button type="button" @click="cancelList">取消動作</button>
      <div v-if="action === 'add'">
        <div style="margin: 12px 0;">
          品項 : <input type="text" v-model="tempAddItem.title"   />
          描述 : <input type="text" v-model="tempAddItem.describe" />
        </div>
        <div style="margin: 12px 0;">
          價格 : <input type="number" v-model="tempAddItem.price" />
          庫存 : <input type="number" v-model="tempAddItem.inventory" />
        </div>
      </div>
    </div>
    <table>
      <thead>
        <tr>
          <th scope="col">品項</th>
          <th scope="col">描述</th>
          <th scope="col">價格</th>
          <th scope="col">庫存</th>
        </tr>
      </thead>
      <tbody >
        <tr v-for="item in drinkList" :key="item.id">
          <td>
            <input type="text" v-model="item.title" :disabled="!isEditing" :class="{ 'input-no-border': !isEditing, 'strikethrough': item.isDeleted }" />
          </td>
          <td>
            <input type="text" v-model="item.describe" :disabled="!isEditing" :class="{ 'input-no-border': !isEditing, 'strikethrough': item.isDeleted }" />
          </td>
          <td>
            <input type="number" v-model="item.price" :disabled="!isEditing" :class="{ 'input-no-border': !isEditing, 'strikethrough': item.isDeleted }" />
          </td>
          <td>
            <input type="number" v-model="item.inventory" :disabled="!isEditing" :class="{ 'input-no-border': !isEditing, 'strikethrough': item.isDeleted }" />
          </td>
          <td class="border-none">
            <button type="button" class="btn" v-if="isDeleting" @click="deleteItem(item)">x</button>
            <button type="button" class="btn" v-if="isDeleting" @click="cancelDeleteItem(item)">o</button>
            <button type="button" class="btn" v-if="!isDeleting" @click="decreaseInventory(item)">-</button>
            <button type="button" class="btn" v-if="!isDeleting" @click="increaseInventory(item)">+</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
  table {
    width: 100%;
    table-layout: auto;
    margin-top: 12px;
    /* border-collapse: collapse; */
  }

  th, td {
    border: 1px solid #000; 
    padding: 6px;
    text-align: left; 
    width: auto;
  }

  th {
    background-color: #c8e9d7;
    font-weight: bold;
  }

  input {
    margin: 0;
  }

  .btn {
    margin: 0 10px 0 0;
    width: 30px;
    height: 30px;
  }

  .text-center {
    text-align: center; 
  }
  
  .border-none {
    border: none;
  }

  .strikethrough {
    text-decoration: line-through;
  }

  .input-no-border {
    border: none;
    outline: none;
    background-color: transparent;
  }
</style>
