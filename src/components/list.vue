<script setup>
import { reactive } from 'vue'
import Autocomplete from './autocomplete.vue'

const inputList = reactive([])

const fetchFromServer = async () => {
  try {
    const response = await fetch('https://run.mocky.io/v3/0be4a97b-6818-4b93-8451-16147bc1f710')
    if (!response.ok) {
      throw response
    }
    const data = await response.json()

    // updating inputList
    data.forEach((item) => {
      /**
         * {
  "name": "Q01",
  "type": "choice-autocomplete",
  "prompt": "What is your highest level of education?",
  "value": "Associates",
  "placeholder": "Select",
  "status": "EDITABLE",
  "required": true,
  "criteria": [],
  "options": [
    {
      "label": "Less than High School",
      "value": "Other"
    },
  ]
}
         */
      inputList.push(item)
    })
  } catch (error) {
    console.log('Error in fetching from server', error)
  }
}

fetchFromServer()
</script>

<template>
  <div class="list-wrapper">
    <div v-for="input in inputList" :key="input.name" class="list-item">
      <div v-if="input.type === 'choice-autocomplete'" class="autocomplete">
        <Autocomplete :id="input.name" :inputProp="input" />
      </div>
      <div v-else>Unsupported</div>
    </div>
  </div>
</template>

<style scoped>
.list-item:nth-child(n) {
  margin-bottom: 5px;
  padding: 10px;
  border-bottom: 1px solid #a2a2a2;
}
</style>
