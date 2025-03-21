<template>
    <div class = "backdrop">
        <div class = "modal">
            <h2>Enter your fish details</h2>
            <p>Species:</p>
            <input type = "text" v-model="inputSpecies">
            <p>Weight(lbs):</p>
            <input type = "text" v-model="inputWeight">
            <p>Date(MM/DD/YYYY):</p>
            <input type = "text" v-model="inputDate">
            <br>
            <button @click = "closeModal()">Confirm</button>
            <button @click ="closeModal()">Cancel</button>
        </div>
    </div>
</template>

<script setup>
import {ref, defineEmits} from "vue"

let inputSpecies = ref("")
let inputDate = ref("")
let inputWeight = ref("")

const emit = defineEmits('close')
const closeModal = () => {
    emit('close')
}


</script>

<style scoped>
.backdrop {
    top: 0;
    position: fixed;
    background: rgba(0, 0, 0, 0.5);
    width: 100%;
    height: 100%;
}

.modal {
    width: 400px;
    padding: 20px;
    margin: 100px auto;
    background: white;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    gap: 10px;
    color: black;
}
</style>