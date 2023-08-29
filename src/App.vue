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
const toggleDark = ref(false)
const filterGroupWorks = computed(() => {
  return groupworks.value.filter((element) => {
    return element.projectName.toLowerCase().includes(searchValue.value.toLowerCase())
  })
})
</script>

<template>
  <div class="w-full min-h-screen">
    <!-- Header -->
    <div class="w-full flex p-3" :class="[toggleDark ? 'dark-mode' : 'light-mode']">
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
      <div class="flex justify-between items-center">
        <h3>dark mode</h3>
        <input type="checkbox" v-model="toggleDark" class="checkbox" id="checkbox">
        <label for="checkbox" class="checkbox-label flex justify-center items-center">
          <span class="ball"></span>
        </label>
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
      <div class="p-5 grid grid-cols-7 text-md font-semibold " :class="[toggleDark ? 'dark-mode' : 'light-mode']">
        <h3>Section</h3>
        <h3>Group Name</h3>
        <h3 class="col-span-3">GitHub Repository</h3>
        <h3 class="col-span-2">Group Members</h3>
      </div>
      <!-- table body -->
      <div class="p-5 grid grid-cols-7 text-xs" v-for="(group, index) in filterGroupWorks" :key="index"
      :class="[
    !toggleDark && index % 2 === 0 ? 'light-row' : '',
    !toggleDark && index % 2 !== 0 ? 'lighter-row' : '',
    toggleDark && index % 2 === 0 ? 'darker-row' : '',
    toggleDark && index % 2 !== 0 ? 'dark-row' : ''
  ]">
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
    <div class="flex flex-col gap-4 justify-center items-center " :class="[toggleDark ? 'dark-mode' : 'light-mode']">
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
        <select v-model="status" class=" mr-3 ml-3 border border-gray-300 rounded-lg p-0.5 w-1/2">
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

<style scoped>
.checkbox {
  opacity: 0;
  position: absolute;
}

.checkbox-label {
  background-color: #111;
  width: 50px;
  height: 26px;
  border-radius: 50px;
  position: relative;
  padding: 5px;
  cursor: pointer;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  transition: background-color 0.2s ease-in-out; /* Add transition */
}

.checkbox-label .ball {
  background-color: #fff;
  width: 21px;
  height: 21px;
  position: absolute;
  left: 1px;
  right: 1px; /* Adjusted positioning */
  border-radius: 50%;
  transition: transform 0.2s linear, background-color 0.2s linear; ;
}

.checkbox:checked + .checkbox-label .ball {
  transform: translateX(20px);
  background-color: black; 
}

/* Change background color when the circle is on the left */
.checkbox:checked + .checkbox-label {
  background-color: white;
}

/* Set the border to black */
.checkbox-label {
  border: 2px solid #000;
}

.dark-mode {
  background-color: black;
  color: white;
  transition: transform 0.2s linear, background-color 0.2s linear, color 0.2s linear;
}


/* Darker background color for odd rows in dark mode */
.darker-row {
  background-color: #333;
  color: white;
  transition: transform 0.2s linear, background-color 0.2s linear, color 0.2s linear;
}



/* Lighter background color for even rows in dark mode */
.dark-row {
  background-color: #444;
  color: white;
  transition: transform 0.2s linear, background-color 0.2s linear, color 0.2s linear;
}

.light-mode {
  background-color: white;
  color: black;
  transition: transform 0.2s linear, background-color 0.2s linear, color 0.2s linear;
}


.light-row{
  background-color: rgba(128, 128, 128, 0.203);
  color: black;
  transition: transform 0.2s linear, background-color 0.2s linear, color 0.2s linear;
}

.lighter-row{
  background-color: white ;
  color: black;
  transition: transform 0.2s linear, background-color 0.2s linear, color 0.2s linear;
}
</style>