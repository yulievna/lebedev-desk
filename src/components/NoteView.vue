<template>
    
</template>
<script>
export default {
    name: "Note",
    setup() {
      const notes = ref([
        {id: 0, color: "red", isPin: false},
        {id: 1, color: "red", isPin: false},
        {id: 2, color: "red", isPin: false},
      ])
      const startDrag = (event, item) => {
        event.dataTransfer.dropEffect = 'move'
        event.dataTransfer.effectAllowed = 'move'
        event.dataTransfer.setData('noteID', note.id)
      }
      const onDrop = (event, list) => {
        const noteID = event.dataTransfer.getData('noteID')
        const note = note.value.find((item) => note.id = noteID)
      }
      return {
        startDrag,
        notes,
        onDrop
      }
    }
}
</script>
