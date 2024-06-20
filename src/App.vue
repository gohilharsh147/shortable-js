<script setup lang="ts">
import { Sortable } from 'sortablejs-vue3'
import { ref, watch } from 'vue'

interface Item {
  id: number
  name: string | null
  subItems: subItem[]
}

interface subItem {
  subItemId: number
  subItemName: string | null
}

const sectionName = ref<string | null>(null)
const items = ref<Item[]>([
  {
    id: 1,
    name: 'Fresh Produce',
    subItems: [
      {
        subItemId: 1,
        subItemName: 'Apples'
      },
      {
        subItemId: 2,
        subItemName: 'Bananas'
      },
      {
        subItemId: 3,
        subItemName: 'Mangoes'
      },
      {
        subItemId: 4,
        subItemName: 'Pineapples'
      }
    ]
  },
  {
    id: 2,
    name: 'Dairy',
    subItems: []
  },
  {
    id: 3,
    name: 'Meat',
    subItems: []
  },
  {
    id: 4,
    name: 'Bakery',
    subItems: [
      {
        subItemId: 1,
        subItemName: 'White Bread'
      },
      {
        subItemId: 2,
        subItemName: 'Whole Wheat Bread'
      }
    ]
  },
  {
    id: 5,
    name: 'Canned',
    subItems: []
  }
])

const storeSection = () => {
  items.value.unshift({
    id: items.value[items.value.length - 1].id || 0,
    name: sectionName.value,
    subItems: []
  })
  sectionName.value = ''
}
</script>

<template>
  <div class="container mx-auto">
    <div class="heading my-7">
      <h1 class="text-4xl lg:text-4xl mb-2 lg:mt-4">Grocery Store</h1>
      <p class="text-md text-gray-500">Find this project useful? You can buy me a ☕ or a 🍺.</p>
    </div>

    <form @submit.prevent="storeSection()">
      <label for="search" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white"
        >Search</label
      >
      <div class="relative">
        <input
          type="text"
          class="block shadow-none w-full p-4 border-0 text-sm text-gray-900 border-gray-300 rounded-lg bg-gray-50"
          placeholder="Enter Section Name"
          required
          v-model="sectionName"
        />
        <button
          type="submit"
          class="text-white absolute end-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none font-medium rounded-lg text-sm px-4 py-2"
        >
          Add Section
        </button>
      </div>
    </form>

    <Sortable
      v-if="items.length > 0"
      :list="items"
      itemKey="id"
      :options="{ animation: 150, handle: '.draggable' }"
    >
      <template #item="{ element, index }">
        <div class="draggable" :key="element.id">
          <a
            class="my-3 block p-6 bg-white border border-gray-200 rounded-lg shadow hover:bg-gray-100 cursor-pointer"
          >
            <h5 class="text-2xl font-bold tracking-tight text-gray-900">
              {{ element.name }}
            </h5>

            <div class="sub-items" v-if="element.subItems && element.subItems.length > 0">
              <Sortable
                :list="element.subItems"
                itemKey="subItems"
                :options="{ animation: 150, handle: '.draggable' }"
              >
                <template #item="{ element, index }">
                  <div class="draggable" :key="index">
                    <a
                      class="my-3 block p-3 bg-white border border-gray-200 rounded-lg shadow hover:bg-gray-100 cursor-pointer"
                    >
                      <h5 class="text-2xl font-bold tracking-tight text-gray-900">
                        {{ element.subItemName }}
                        <!-- Make sure this property exists -->
                      </h5>
                    </a>
                  </div>
                </template>
              </Sortable>
            </div>

            <p class="mt-3 text-gray-500" v-else>
              There are no items available for <b>{{ element.name }}</b>
            </p>
          </a>
        </div>
      </template>
    </Sortable>

    <p class="text-center my-5 text-gray-500" v-else>
      There are no sections available at the moment.
    </p>
  </div>
</template>

<style scoped></style>
