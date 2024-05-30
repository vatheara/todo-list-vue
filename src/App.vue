<script setup lang="ts">
type Item = {
  id: string
  name: string
  completed: boolean
}
import { ref } from 'vue'
const items = ref<Item[]>([])

const name = ref('')

const handleAdd = () => {
  if (!name.value) return

  items.value.push({
    id: Date.now().toString(),
    name: name.value,
    completed: false
  })
  name.value = ''
}

const handleRemove = (id: string) => {
  const tmp = items.value.filter((item) => item.id !== id)
  items.value = tmp
}

const handleDone = (id: string) => {
  items.value.forEach((item) => {
    if (item.id === id) {
      item.completed = !item.completed
    }
  })
}
</script>

<template>
  <div class="main">
    <div class="title">TODO List</div>
    <div class="item-wrapper">
      <ul class="list">
        <li v-for="item in items" :key="item.name" :class="{ done: item.completed }">
          {{ item.name }} - {{ item.completed ? 'completed' : 'not completed' }} |
          <button @click="handleRemove(item.id)">remove</button>
          <button @click="handleDone(item.id)">{{ item.completed ? 'undo' : 'done' }}</button>
        </li>
      </ul>
      <div class="add-todo">
        <input v-model="name" />
        <button class="add-btn" @click="handleAdd">Add</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.title {
  font-size: larger;
  font-weight: bold;
}

.item-wrapper {
  display: flex;
  flex-direction: column;
}

.add-todo {
  display: flex;
  justify-content: center;
  margin-top: 8px;
}

.add-btn {
  margin-left: 6px;
}

.done {
  text-decoration: line-through;
}
</style>
