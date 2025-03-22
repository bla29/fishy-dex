<script setup>
import Modal from './components/Modal.vue'
import {ref, onMounted} from 'vue'

let modal = ref(false)
let edit = ref(false)
let fishIdStore = ref(0)
let fishList = ref([])

const toggleModal = async() => {
  modal.value = !modal.value
  if(!modal.value) {
    console.log(!modal.value)
    fishList.value = []
    await getFishList()
  }
}

const toggleeditModal = async() => {
  edit.value = !edit.value
  if(!edit.value) {
    console.log(!edit.value)
    fishList.value = []
    await getFishList()
  }
}

const storeFishId = (fishId) => {
  console.log(fishId)
  fishIdStore.value = fishId
  toggleeditModal()
}

const getFishList = () => {
  fetch('http://localhost:3000/fish', {
    method: 'GET',
    headers: {
      'Accept': 'application/json',  // Ensure server returns JSON
      'Content-Type': 'application/json'
    }
  })
    .then(res => res.json())
    .then(data => fishList.value = [...data])
    .catch(err => console.log(err.message))
}

const deleteFish = (id) => {
  fetch('http://localhost:3000/fish/' + id, {
    method: 'DELETE',
    headers: {
      'Accept': 'application/json',  // Ensure server returns JSON
      'Content-Type': 'application/json'
    }
  })
    .then(res => res.json())
    .catch(err => console.log(err.message))
    
    const index = fishList.value.findIndex(fish => fish.id == id)
    fishList.value.splice(index, 1)
}

onMounted(() => {
  getFishList()
})
  
</script>

<template>
  <header>
    <div class="container">
      <img src="@/assets/bass.png" width="200" height="200" />
      <div>
        <h1 class="green">Fishy-Dex</h1>
        <h3>
          A collection of your fish catches.
        </h3>
      </div>
    </div>
    <button class = "button" @click = "toggleModal()">Log your catch!</button>
    <div v-if = "modal">
      <Modal addHeader="Add your fish details" :edit="edit" @closeAddModal = "toggleModal()" />
    </div>
    <div v-if = "edit">
      <Modal editHeader="Edit your fish details" :edit="edit" :fishIdStore = "fishIdStore"  @closeEditModal = "toggleeditModal()" />
    </div>
    </header>


  <body>
    <ul>
      <li v-for="fish in fishList" :key="fish.id" class="fishlist">
        <p>Species: {{ fish.species }}</p>
        <p>Weight: {{ fish.weight }}</p>
        <p>Catch Date: {{ fish.date }}</p>
        <button @click = "storeFishId(fish.id)">Edit catch</button>
        <button @click = "deleteFish(fish.id)">Delete catch</button>
      </li>
    </ul>
  </body>


</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
  text-align: center;
}

.button {
  margin: 50px;
  padding: 10px;
}

.container {
  align-self: center;
}

.fishlist {
  padding: 10px;
}


</style>
