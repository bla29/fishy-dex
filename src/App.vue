<script setup>
import Modal from './components/Modal.vue'
import {ref} from 'vue'

let modal = ref(false)
let fishList = []

const toggleModal = () => {
  modal.value = !modal.value
  console.log (modal.value)
}
  
fetch('http://localhost:3000/fish', {
  method: 'GET',
  headers: {
    'Accept': 'application/json',  // Ensure server returns JSON
    'Content-Type': 'application/json'
  }
})
  .then(res => res.json())
  .then(data => fishList.push(...data))
  .catch(err => console.log(err.message))

  
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
      <Modal @close = "toggleModal()" />
    </div>
    </header>

  <body>
    <ul>
      <li v-for="fish in fishList" :key="fish.id" class="fishlist">
        <p>Species: {{ fish.species }}</p>
        <p>Weight: {{ fish.weight }}</p>
        <p>Catch Date: {{ fish.date }}</p>
        <button>Edit catch</button>
        <button>Delete catch</button>
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
