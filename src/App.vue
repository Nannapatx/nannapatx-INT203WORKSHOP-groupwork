<script setup>

import { groups } from './data/groupworks.js'
import IconSearch from './components/icons/IconSearch.vue'
import { ref, computed } from 'vue'

const section = ref('')
const groupworks = ref(groups)
const searchValue = ref('')
const notes = ref('')
const status = ref('')
const accept = ref(false)
const roles = ref([])
const filterGroupWorks = computed(()=>{
  return groupworks.value.filter((element)=>{
    return element.projectName.toLowerCase().includes(searchValue.value.toLowerCase())
})
})
</script>

<template>
  <div class="w-full min-h-screen">
    <!-- Header -->
    <div class="w-full flex p-3">
      <!-- left panel -->
      <div class="w-1/2">
        <div class="flex flex-row space-x-2">
          <img src="./components/icons/vuelogo.ico" class="h-10 w-15" />
          <div class="flex flex-col">
            <p class="tracking-widest">Vue Group Projects</p>
            <p class="italic text-xs">
              Part of Learning Activities for INT203 Client-Side Programming II
            </p>
          </div>
        </div>
      </div>
      <!-- right panel -->
      <div class="w-1/2">
        <div class="flex space-x-3 justify-end">
          <IconSearch class="self-center" />
          <input v-model="searchValue" type="text" class="w-1/2 p-1 border border-gray-200 rounded-lg" />
        </div>
      </div>
    </div>
    <!-- body -->
    <div class="w-full">
      <!-- table heading -->
      <div class="p-5 grid grid-cols-7 text-md font-semibold">
        <h3>Section</h3>
        <h3>Group Name</h3>
        <h3 class="col-span-3">GitHub Repository</h3>
        <h3 class="col-span-2">Group Members</h3>
      </div>
      <!-- table body -->
      <div class="p-5 grid grid-cols-7 text-xs" v-for="(group, index) in filterGroupWorks" :key="index"
        :class="index % 2 === 0 ? 'bg-gray-100' : 'bg-white'">
        <p>{{ group.section }}</p>
        <p>{{ group.projectName }}</p>
        <p class="col-span-3">{{ group.githubRepo }}</p>
        <div class="col-span-2">
          <div v-for="student in group.students" :key="student.id">
            <span> {{ student.id }} {{ student.name }}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="flex flex-col gap-4 justify-center items-center ">
      <h1 class="mt-6 self-center font-bold text-2xl ">Adding a New Groupwork</h1>
      <div class="flex justify-between">
        <h3 class="mr-3">Section: </h3>
        <input v-model.trim="section" type="text" class="w-[200px] p-1 border border-gray-200 rounded-lg" />
      </div>
      <div class="flex justify-between">
        <h3 class="mr-3">Notes: </h3>
        <textarea v-model.trim="notes" type="text" class="w-[300px] h-[200px] p-1 border border-gray-200 rounded-lg">
        </textarea>
      </div>

      <div class="flex justify-between">
          Status:
          <select
            v-model="status"
            class=" mr-3 ml-3 border border-gray-300 rounded-lg p-0.5 w-1/2"
          >
            <option value="leader">leader</option>
            <option value="member">member</option>
          </select>
          <span>{{ status }}</span>
        </div>
        <div>
          <input type="checkbox" v-model="accept" />
          You accept a requirement of group work
          <span>({{ accept }})</span>
        </div>

        <div class="flex space-x-2">
          <label for="">Select your role:</label>

          <input type="checkbox" value="Programming" v-model="roles" />
          <label for="">Programming</label>

          <input type="checkbox" value="UX/UI" v-model="roles" />
          <label for="">UX/UI</label>

          <input type="checkbox" value="Testing" v-model="roles" />
          <label for=""> Testing</label>
        </div>
        <span>{{ roles }}</span>
      </div>
    </div>
</template>

<style scoped></style>