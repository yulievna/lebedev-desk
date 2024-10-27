<template>
  <div class="desk">
    <div class="note" v-for="note in notes" :key="note.id"
      :style="{ background: note.color, left: note.x + 'px', top: note.y + 'px' }" @mousedown="startDrag($event, note)">
      <div class="menu" v-if="note.menu">
        <button class="button btn-pin" @click="pinNote(note)"><img src="../public/pin-icon.svg" alt="pin"></button>
        <button class="button btn-delete" @click="deleteNote(note.id)"><img src="../public/delete-icon.svg"
            alt="delete"></button>
      </div>
      <div class="pin" v-if="note.pin"></div>
      <textarea class="text" v-model="note.text" @click="note.menu = !note.menu"></textarea>
    </div>
    <div class="add">
      <button class="add-note add-note__red" @click="createNote('#FEB1B1')"></button>
      <button class="add-note add-note__blue" @click="createNote('#B1D0FE')"></button>
      <button class="add-note add-note__green" @click="createNote('#A1E0B4')"></button>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue';

const notes = ref([]);
let draggingNote = ref(null);
let offsetX = ref(0);
let offsetY = ref(0);

function createNote(color) {
  notes.value.push({
    id: notes.value.length + 1,
    color: color,
    text: '',
    pin: false,
    menu: false,
    x: 100,
    y: 50
  });
}
watch(notes, newValue => {
  localStorage.setItem('notes', JSON.stringify(newValue))
}, { deep: true })

function deleteNote(id) {
  notes.value = notes.value.filter(item => item.id !== id)
}

function pinNote(note) {
  note.pin = !note.pin
  note.menu = !note.menu
}

function startDrag(event, note) {
  draggingNote.value = note
  if (!note.pin) {
    const rect = event.target.getBoundingClientRect()
    offsetX.value = event.clientX - rect.left
    offsetY.value = event.clientY - rect.top
    document.addEventListener('mousemove', onMouseMove)
    document.addEventListener('mouseup', stopDrag)
  }
}

function onMouseMove(event) {
  if (!draggingNote.value) return;
  draggingNote.value.x = event.clientX - offsetX.value;
  draggingNote.value.y = event.clientY - offsetY.value;
}

function stopDrag() {
  draggingNote.value = null;
  document.removeEventListener('mousemove', onMouseMove);
  document.removeEventListener('mouseup', stopDrag);
}
onMounted(() => {
  notes.value = JSON.parse(localStorage.getItem('notes')) || []
})
</script>

<style>
.desk {
  width: 1200px;
  height: 690px;
  background: #F6F6F6;
  background-image: radial-gradient(circle at 3px 3px, #D9D9D9 3px, transparent 0);
  background-size: 30px 30px;
  border-radius: 50px;
  filter: drop-shadow(0 0 15px #D9D9D9);
  position: relative;
  min-height: 10px;
}

.note {
  position: absolute;
  top: calc(50% - 125px);
  left: calc(50% - 125px);
  width: 250px;
  height: 250px;
}

.pin {
  position: absolute;
  top: -20px;
  left: 50px;
  width: 150px;
  height: 40px;
  background-color: #DBCDFF;
  border: 4px solid #fff;
  outline: none;
}

.text {
  width: 100%;
  height: 100%;
  border: none;
  padding: 25px 20px;
  background: transparent;
  resize: none;
  font-family: 'Open Sans', sans-serif;
  color: #000;
  font-size: 15px;
  overflow: hidden;
}

.menu {
  position: absolute;
  top: -40px;
  left: 90px;
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 70px;
  height: 35px;
  border-radius: 6px;
  background-color: #222222;
}

button {
  border: none;
  background: none;
}

.text:focus {
  outline: none;
}

.add {
  width: 200px;
  height: 70px;
  display: flex;
  justify-content: center;
  position: absolute;
  bottom: 35px;
  left: 35px;
  align-items: center;
  gap: 20px;
  background-color: #F6F6F6;
  border: 2px solid #D9D9D9;
  border-radius: 10px;
}

.add-note {
  width: 35px;
  height: 35px;
  transition: 0.5s;
}

.add-note__red {
  background-color: #FEB1B1;
}

.add-note__blue {
  background-color: #B1D0FE;
}

.add-note__green {
  background-color: #A1E0B4;
}

.add-note:hover {
  margin-bottom: 10px;
}
</style>