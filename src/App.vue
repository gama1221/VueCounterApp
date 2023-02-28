<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea name="note" id="note" v-model="newNote" cols="30" rows="10" placeholder="Write your note here"
          autofocus>
                  </textarea>
        <button @click="addNote()">Add Note</button>
        <button class="close" @click="showModal = false, errorMessage='', newNote=''">Close</button>
        <p v-show="errorMessage" style="color:brown">{{ errorMessage }}</p>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button class="plus" @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div class="card" v-for="note in notes" :key="note.id" :style="{ backgroundColor: note.color }" @mouseover="handleEditing()" @mouseleave="handleEditingClose()">
          <button v-if="isEditting">
            <span>Edit</span>
          </button>
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">
            {{ note.date }}
          </p>
        </div>
      </div>
    </div>
  </main>
</template>
<script setup>
import { ref } from 'vue';
const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errorMessage = ref("");
const isEditting = ref(false);
const addNote = () => {
  if (newNote.value.length < 10) return errorMessage.value = "At least 10 characters are allowed";
  notes.value.push({
    id: Math.random(1000000),
    text: newNote.value,
    color: getRandomColor(),
    date: new Date().toLocaleDateString()
  })
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
}
const handleEditing = ()=>{
  isEditting.value = true;
}
const handleEditingClose = ()=>{
  isEditting.value = false;
}
const getRandomColor = () => {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}
const dt = new Date().toLocaleTimeString();
</script>
<style scoped>
main {
  background-color: aliceblue;
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

button.plus {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  height: 100%;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.modal .close {
  background-color: red;
  margin-top: 7px;
}
</style>