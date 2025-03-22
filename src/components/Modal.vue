<template>
    <div class = "backdrop">
        <div class = "modal">
            <h2 v-if="edit">{{ editHeader }}</h2>
            <h2 v-else> {{ addHeader }}</h2>
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
            <button @click = "confirmButton(edit, fishIdStore)">Confirm</button>
            <button @click ="closeModal(edit)">Cancel</button>
        </div>
    </div>
</template>

<script setup>
import {ref, defineEmits} from "vue"

defineProps({
  editHeader: {
    type: String,
    required: false
  },
  addHeader: {
    type: String,
    required: false
  },
  edit: {
    type: Boolean,
    required: true
  },
  fishIdStore: {
    type: Number,
    required: false
  }
});

let inputSpecies = ref("")
let inputDate = ref("")
let inputWeight = ref("")

let errorMessage = ref(false)

const emit = defineEmits(['closeAddModal', 'closeEditModal'])

const closeModal = (edit) => {
    if (edit) {
        emit('closeEditModal')
    }
    else {
        emit('closeAddModal')
    }
}

const confirmButton = async(edit, fishIdStore) => {
    if (inputSpecies.value && inputDate.value && inputWeight.value) {
        const payload = {
                species: inputSpecies.value.toString(),
                weight: Number(inputWeight.value),
                date: inputDate.value.toString()
        }
        if(edit) {
            await fetch('http://localhost:3000/fish/' + fishIdStore, {
                method: 'PATCH',
                headers: {
                    'Accept': 'application/json',  // Ensure server returns JSON
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(payload)
            })
            .then(res => res.json())
            .then(data => console.log(data))
            .catch(err => console.log(err.message))
            closeModal(edit)
        }
        else {
            await fetch('http://localhost:3000/fish', {
                method: 'POST',
                headers: {
                    'Accept': 'application/json',  // Ensure server returns JSON
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(payload)
            })
            .then(res => res.json())
            .then(data => console.log(data))
            .catch(err => console.log(err.message))
            closeModal(edit)
        }
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