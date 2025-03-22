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
  fetch('https://fishy-dex-store-production.up.railway.app/fish', {
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
  fetch('https://fishy-dex-store-production.up.railway.app/fish/' + id, {
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
  <div class = "container">
    <div>
      <img src="@/assets/bass.png" width="200" height="200" />
      <div>
        <h1 class="green">Fishy-Dex</h1>
        <h3>
          A collection of your fish catches.
        </h3>
      </div>
    </div>
    <button class = "buttonHeader" @click = "toggleModal()">Log your catch!</button>
    <div v-if = "modal">
      <Modal addHeader="Add your fish details" :edit="edit" @closeAddModal = "toggleModal()" />
    </div>
    <div v-if = "edit">
      <Modal editHeader="Edit your fish details" :edit="edit" :fishIdStore = "fishIdStore"  @closeEditModal = "toggleeditModal()" />
    </div>
      
    <div>
      <ul>
        <li v-for="fish in fishList" :key="fish.id" class="fishlist">
          <p>Species: {{ fish.species }}</p>
          <p>Weight: {{ fish.weight }}</p>
          <p>Catch Date: {{ fish.date }}</p>
          <button @click = "storeFishId(fish.id)" class="buttonList">Edit catch</button>
          <button @click = "deleteFish(fish.id)" class="buttonList">Delete catch</button>
        </li>
      </ul>
    </div>
</div>

</template>

<style scoped>

.buttonHeader {
  margin: 25px;
  padding: 10px;
}

.buttonList {
  margin: 5px;
}

.container {
  width: 100%;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  
}

.fishlist {
  padding: 10px;
}



</style>
