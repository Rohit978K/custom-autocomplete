<script setup>
import { computed, ref, shallowRef } from 'vue'
const props = defineProps({
  inputProp: {
    type: Object,
    required: true,
  },
})

const { name, type, prompt, value, placeholder, status, required, criteria, options } =
  props.inputProp

const localValue = ref(value)
const localOptions = computed(() => {
  let list = []
  if (localValue.value?.trim()?.length) {
    const lowerCaseValue = localValue.value.trim().toLowerCase()
    list = options.filter((item) => item.value.toLowerCase().includes(lowerCaseValue))
  } else {
    list = options
  }
  return list
})

const showOptions = shallowRef(false)

const hideOptionsLazy = () => {
  setTimeout(() => {
    showOptions.value = !showOptions.value
  }, 100)
}
</script>

<template>
  <div>
    <label class="wrapper" :for="name">
      {{ prompt }}
      <input
        :id="name"
        ref="input"
        v-model="localValue"
        type="text"
        @focus="showOptions = !showOptions"
        @blur="hideOptionsLazy"
      />
      <transition>
        <div v-if="showOptions" class="options">
          <ul>
            <li v-for="item in localOptions" @click.prevent="localValue = item.value">
              {{ item.label }}
            </li>
          </ul>
        </div>
      </transition>
    </label>
  </div>
</template>

<style>
.wrapper {
  display: flex;
  flex-direction: column;
}

input {
  padding: 0.5rem;
  font-size: 1.1rem;
  border-radius: 5px;
  outline: none;
}

.options {
  box-shadow: 2px 1px 10px #a3a3a3;
  border-radius: 5px;
  overflow: hidden;
}

.options ul {
  padding: 0.8rem;
}

li::marker {
  content: '';
}
</style>
