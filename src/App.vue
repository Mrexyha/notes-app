<script setup>
import { ref } from "vue";

const showModal = ref(false);
const changeCard = ref(false);

const newNote = ref("");
const newHeader = ref("");

const errorMessage = ref("");

const notes = ref([]);

const currentHeader = ref("");
const currentNote = ref("");

let curHeader = "";
let curNote = "";

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = "Note needs to be 10 characters or more!");
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    header: newHeader.value,
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });

  showModal.value = false;
  newNote.value = "";
  newHeader.value = "";
  errorMessage.value = "";
};

const editNote = (note) => {
  currentHeader.value = note.header;
  currentNote.value = note.text;
  changeCard.value = true;

  curHeader = currentHeader.value;
  curNote = currentNote.value;
};

const saveChanges = () => {
  if (currentNote.value.length < 10) {
    return (errorMessage.value = "Note needs to be 10 characters or more!");
  }

  let index = -1;

  for (let i = 0; i < notes.value.length; i++) {
    if (
      notes.value[i].header === curHeader &&
      notes.value[i].text === curNote
    ) {
      index = i;
    }
  }

  if (index !== -1) {
    notes.value[index].header = currentHeader.value;
    notes.value[index].text = currentNote.value;
    notes.value[index].date = new Date();
  }

  currentHeader.value = "";
  currentNote.value = "";
  errorMessage.value = "";
  changeCard.value = false;
};
</script>

<template>
  <main>
    <div v-if="changeCard" class="overlay">
      <div class="modal">
        <input
          v-model.trim="currentHeader"
          class="title"
          type="text"
          placeholder="Edit title"
        />
        <textarea
          v-model.trim="currentNote"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="saveChanges">Save Changes</button>
        <button class="close" @click="changeCard = false">Close</button>
      </div>
    </div>
    <div v-if="showModal" class="overlay">
      <!-- <div v-show="showModal" class="overlay"> -->
      <div class="modal">
        <input
          v-model.trim="newHeader"
          class="title"
          type="text"
          placeholder="Add a title"
        />
        <textarea
          v-model.trim="newNote"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="card-container">
        <div
          @click="editNote(note)"
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <h1 class="main-header">{{ note.header }}</h1>
          <div class="card-content">
            <p class="main-text">{{ note.text }}</p>
            <p class="date">{{ note.date.toLocaleDateString("uk-UA") }}</p>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
  word-wrap: break-word;
  line-height: 1.2;
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
header button {
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

.card-content {
  height: 225px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
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
  background-color: rgb(249, 70, 70);
  margin-top: 7px;
}

.modal p {
  color: red;
}

.main-header {
  font-size: 24px;
  margin-bottom: 10px;
  display: flex;
  flex-wrap: wrap;
}
.main-text {
  max-height: 135px;
  overflow: hidden;
  position: relative;
  cursor: pointer;
}

.title {
  border: none;
  margin-bottom: 25px;
  font-size: 25px;
}
</style>
