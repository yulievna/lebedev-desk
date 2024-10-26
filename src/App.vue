<template>
  <div class="desk">
    <div class="note" 
      v-for="note in notes" :key="note.id"
      :style="{ background: note.color, left: note.x + 'px', top: note.y + 'px' }" 
      @mousedown="startDrag($event, note)"
    >
      <div class="menu" v-if="note.menu">
        <btn class="button btn-pin" @click="pinNote(note)"><img src="../../public/pin-icon.svg" alt="pin"></btn>
        <btn class="button btn-delete" @click="deleteNote(note.id)"><img src="../../public/delete-icon.svg" alt="delete"></btn>
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

<script>
export default {
  data() {
    return {
      notes: [],
      draggingNote: null,
      offsetX: 0,
      offsetY: 0,
    }
  },
  methods: {
    createNote(color) {
      this.notes.push({ id: this.notes.length + 1, color: color, text: 'dsad', pin: false, menu: false, x: 100, y: 50 })
    },
    deleteNote(id) {
      this.notes.splice(id - 1, 1);
    },
    pinNote(note){
      note.pin = !note.pin
      note.menu = !note.menu
      console.log(notes[id].pin, notes[id].menu);
      
    },
    startDrag(event, note) {
      this.draggingNote = note;
      if(!note.pin){
        const rect = event.target.getBoundingClientRect();
        this.offsetX = event.clientX - rect.left;
        this.offsetY = event.clientY - rect.top;
        document.addEventListener('mousemove', this.onMouseMove);
        document.addEventListener('mouseup', this.stopDrag);
      }
      console.log(note);
      
    },
    onMouseMove(event) {
      if (!this.draggingNote) return;
      this.draggingNote.x = event.clientX - this.offsetX;
      this.draggingNote.y = event.clientY - this.offsetY;
    },
    stopDrag() {
      this.draggingNote = null;
      document.removeEventListener('mousemove', this.onMouseMove);
      document.removeEventListener('mouseup', this.stopDrag);
    },
    unpin(){
      this.draggingNote = null
      document.removeEventListener('mousemove', this.onMouseMove);
      document.removeEventListener('mouseup', this.stopDrag);
      
    }
  },
}
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
  padding-top: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  width: 70px;
  height: 35px;
  border-radius: 6px;
  background-color: #222222;
}

button {
  border: none;
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