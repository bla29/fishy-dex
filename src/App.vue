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

<div class="container">
        <div class="first-div">
            <div class="bottom-awesome-pic">
                <div class="awesome-left-div">
                    <h3 class="hero-main-text">Fishy-Dex</h3>
                    <h1 class="awesome-text">A collection of your fish catches.</h1>
                    <button class="first-button" @click = "toggleModal()">Log your catch!</button>
                </div>
                <img src="@/assets/bass.png" class="first-img">
            </div>
        </div>
        <div v-if = "modal">
          <Modal addHeader="Add your fish details" :edit=false @closeAddModal = "toggleModal()" />
        </div>
        <div v-if = "edit">
          <Modal editHeader="Edit your fish details" :edit=true :fishIdStore = "fishIdStore"  @closeEditModal = "toggleeditModal()" />
        </div>
        <div class="second-div">
            <div class="random-information-header">
                <h4>Catches</h4>
            </div>
            <div class="photo-row">
                <div v-for="fish in fishList" :key="fish.id" class="photo-row-item">
                  <img src="@/assets/fishicon.png" class="photo-row-photo">
                  <li>
                    <p>Species: {{ fish.species }}</p>
                    <p>Weight: {{ fish.weight }}</p>
                    <p>Catch Date: {{ fish.date }}</p>
                    <button @click = "storeFishId(fish.id)" class="buttonList">Edit catch</button>
                    <button @click = "deleteFish(fish.id)" class="buttonList">Delete catch</button>
                  </li>
                </div>
            </div>
        </div>
    </div>

</template>

<style scoped>

html, body {
  margin: 0;
  padding: 0;
  height: 100%; /* Ensure the body and html elements take full height */
}

* {
    font-family: roboto;
}

li {
  list-style-type: none;
}

.first-div {
    background-color: #1F2937;
}

.first-button {
    background-color: #3882F6;
    color: white;
    width: 300px;
    border: none;
    padding: 15px;
    border-radius: 5px;
    font-size:large;
}

.container {
    display: flex;
    flex-direction: column;
}

.top-row-header {
    display: flex;
    justify-content: space-evenly;
    align-items: flex-start;
}

.header-logo-text {
    color: #F9FAF8;
}

.top-header-links {
    display: flex;
    justify-content: space-between;
    gap: 20px;
    color: #E5E7EB;
}


.bottom-awesome-pic {
    display: flex;
    justify-content: space-evenly;
    margin-top: 30px;
    padding-bottom: 60px;
}

.awesome-left-div {
    display: flex;
    flex-direction: column;
    flex-basis: 300px;
    max-height: 300px;
    padding-left: 100px;
}

.hero-main-text {
    color: #F9FAF8;
    font-size: 48px;
    font-weight: 900;
    margin-bottom: 5px;
}

.awesome-text {
    color: #E5E7EB;
    margin-top: 0%;
    margin-bottom: 5px;
}

.first-img {
    max-height: 300px;
    width: 300px;
}

.second-div {
    background-color: white;
}

.random-information-header {
    font-size: 36px;
    color: #1F2937;
    font-weight: 900;
    display: flex;
    justify-content: center;
}

.photo-row {
    display: flex;
    justify-content: center;
    gap: 50px;
}

.photo-row-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    
}

.photo-row-photo {
    max-height: 150px;
    width: 150px;
    border-radius: 10px;
    flex: 1 1 0;
}

.photo-row-text {
    flex: 3 1 0;
    max-width: 150px;
    color: gray;
}

.quote-section {
    background-color: #E5E7EB;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 50px;
}

.quote {
    font-size: 36px;
    font-weight: light;
    color: #1F2937;
    font-style: italic;
    max-width: 700px;
    margin-bottom: 5px;
}

.author-quote {
    font-weight: bold;
    margin-top: 0px;
    padding-left: 500px;
}

.fourth-div {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 100px;
}

.blue-banner {
    background-color: #3882F6;
    display: flex;
    gap: 100px;
    padding-left: 100px;
    padding-right: 100px;
    padding-top: 20px;
    padding-bottom: 20px;
}

.blue-banner-text {
    color: white;
}

.second-button {
    background-color: #3882F6;
    color: white;
    width: 80px;
    border-color: white;
    padding: 5px;
    border-radius: 5px;
    height: 30px;
    align-self: center;
}

.fifth-div {
    background-color: #1F2937;
    display: flex;
    justify-content: center;
    color: white;
    padding: 20px;
}



</style>
