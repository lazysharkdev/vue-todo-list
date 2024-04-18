<script lang="ts" setup>
import { ref, watch } from 'vue'

const props = defineProps({
  initialIsChecked: Boolean
})

const emit = defineEmits(['change'])

const isChecked = ref(props.initialIsChecked)

const toggleCheck = () => {
  isChecked.value = !isChecked.value
}

watch(isChecked, (newVal) => {
  emit('change', newVal)
})
</script>

<template>
  <label :class="{ checked: isChecked }">
    <input type="checkbox" :checked="isChecked" @click="toggleCheck" />
    <slot></slot>
  </label>
</template>

<style scoped>
label {
  cursor: pointer;
}

.checked {
  text-decoration: line-through;
}
</style>
