<script setup>
import Modal from './components/Modal.vue'
import {ref} from 'vue'

let modal = ref(false)

const toggleModal = () => {
  modal.value = !modal.value
  console.log (modal.value)
}
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
    <li>
      hi
    </li>
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


</style>
