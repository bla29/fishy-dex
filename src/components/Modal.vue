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
            <div v-if = "errorMessage">
                <p>Please complete this form before submitting.</p>
            </div>
            <button @click = "confirmButton()">Confirm</button>
            <button @click ="closeModal()">Cancel</button>
        </div>
    </div>
</template>

<script setup>
import {ref, defineEmits} from "vue"

let inputSpecies = ref("")
let inputDate = ref("")
let inputWeight = ref("")

let errorMessage = ref(false)

const emit = defineEmits('close')

const closeModal = () => {
    emit('close')
}

const confirmButton = () => {
    if (inputSpecies.value && inputDate.value && inputWeight.value) {
        emit('close')
    }
    else {
        errorMessage.value = true
    }
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