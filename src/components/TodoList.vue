<script lang="ts" setup>
import { ref, onMounted, computed } from 'vue'
import type { Ref } from 'vue'
import ListItem from './ListItem.vue'

type Item = {
  title: string
  checked: boolean
}

const updateItem = (item: Item): void => {
  const updatedItem = findItemInList(item)
  if (updatedItem) {
    toggleItemChecked(updatedItem)
    setToStorage(storageItems.value)
  }
}

const findItemInList = (item: Item): Item | undefined => {
  return storageItems.value.find((itemInList: Item) => itemInList.title === item.title)
}

const toggleItemChecked = (item: Item): void => {
  item.checked = !item.checked
}

const sortedList = computed(() =>
  [...storageItems.value].sort((a, b) => (a.checked ? 1 : 0) - (b.checked ? 1 : 0))
)

const setToStorage = (items: Item[]): void => {
  localStorage.setItem('list-items', JSON.stringify(items))
}

const getFromStorage = (): Item[] | [] => {
  const stored = localStorage.getItem('list-items')
  if (stored) {
    return JSON.parse(stored)
  }
  return []
}

const storageItems: Ref<Item[]> = ref([])

const initListItems = (): void => {
  console.log('storageItems :>> ', storageItems.value?.length)
  if (storageItems.value?.length === 0) {
    const listItems = [
      { title: 'Make a todo list app', checked: true },
      { title: 'Predict the weather', checked: false },
      { title: 'Play some tunes', checked: false },
      { title: "Let's get cooking", checked: false },
      { title: 'Pump some iron', checked: false },
      { title: 'Track my expenses', checked: false },
      { title: 'Organize a game night', checked: false },
      { title: 'Learn a new language', checked: false },
      { title: 'Publish my work', checked: false }
    ]

    setToStorage(listItems)
    storageItems.value = listItems
  }
}

onMounted(() => {
  initListItems()
  storageItems.value = getFromStorage()
})
</script>

<template>
  <ul>
    <li :key="item.title" v-for="item in sortedList" @click.prevent="updateItem(item)">
      <ListItem :is-checked="item.checked">{{ item.title }}</ListItem>
    </li>
  </ul>
</template>

<style scoped>
ul {
  list-style: none;
}

li {
  margin: 0.4rem 0;
}
</style>
