<template>
  <div
    class="min-h-screen flex flex-col justify-center items-center mx-auto"
  >
    <!-- Main container -->
    <div class="bg-white shadow-lg rounded-lg p-4 border w-4/5">
      <!-- Header -->
      <div class="text-center mb-6">
        <p class="text-2xl font-bold text-gray-800">My Travel List</p>
      </div>

      <!-- Input and Button -->
      <div class="mb-6">
        <div class="flex space-x-2">
          <input
            v-model="inputList"
            @input="handleInput"
            type="text"
            placeholder="Type your items here, separated by commas..."
            class="w-full p-3 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
          />
          <button
            class="bg-blue-500 text-white px-4 py-3 rounded-lg hover:bg-blue-600 transition"
            @click="addNewList"
          >
            Add Item
          </button>
        </div>
      </div>

      <!-- Items List -->
      <div class="mb-6" v-if="items.length > 0">
        <p class="font-semibold text-gray-700 mb-2">I have to check...</p>

        <!-- Grid container -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
          <div
            v-for="(item, index) in items"
            :key="index"
            class="p-3 bg-gray-100 rounded-lg flex justify-between items-center"
          >
            <div class="flex space-x-4 items-center">
              <input type="checkbox" v-model="item.completed" />
              <!-- Conditionally apply 'line-through' class based on 'completed' state -->
              <p :class="item.completed ? 'line-through text-gray-500' : ''">
                {{ item.text }}
              </p>
            </div>

            <button type="submit" @click="removeItem(index)">
              <i class="fa-solid fa-trash-can text-red-600"></i>
            </button>
          </div>
        </div>
      </div>

      <!-- Status Message if no items -->
      <div
        v-if="items.length === 0"
        class="bg-blue-300 px-4 py-2 rounded-lg text-center mb-6"
      >
        <p class="text-white italic">
          Please add items to the package list
        </p>
      </div>

      <!-- Progress -->
      <div
        v-if="items.length > 0 && completedItems !== items.length"
        class="bg-yellow-300 px-4 py-2 rounded-lg text-center mb-6"
      >
        <p class="font-medium text-yellow-600">
          You have completed {{ completedItems }} out of
          {{ items.length }} item{{ items.length === 1 ? '' : 's' }} in the
          package list. ({{ completionPercentage }}%)
        </p>
      </div>

      <!-- Completion Message should be shown if all items are completed -->
      <div
        v-if="items.length > 0 && completedItems === items.length"
        class="bg-green-300 px-4 py-2 rounded-lg text-center"
      >
        <p class="text-green-600 font-semibold">
          Everything is added and completed, ready to go!
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// Initialize input list and items array
const inputList = ref('')
const items = ref([])

// Function to add a new item to the list
const addNewList = () => {
  if (inputList.value.trim()) {
    items.value.push({ text: inputList.value.trim(), completed: false }) // Add new item with 'completed' state set to false
    inputList.value = ''
  }
}

// Function to remove an item by index
const removeItem = index => {
  items.value.splice(index, 1)
}

// Handle input and automatically split by commas
const handleInput = () => {
  // If the input contains a comma, split the string into individual items
  if (inputList.value.includes(',')) {
    const splitItems = inputList.value
      .split(',')
      .map(item => item.trim())
      .filter(item => item !== '')
    // Add each split item to the list
    splitItems.forEach(item => {
      items.value.push({ text: item, completed: false })
    })
    // Clear the input field after processing
    inputList.value = ''
  }
}

// Computed property to get the number of completed items
const completedItems = computed(() => {
  return items.value.filter(item => item.completed).length
})

// Computed property to calculate the completion percentage
const completionPercentage = computed(() => {
  return items.value.length > 0
    ? Math.floor((completedItems.value / items.value.length) * 100)
    : 0
})
</script>

<style scoped></style>
