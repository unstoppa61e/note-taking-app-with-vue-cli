<template>
  <div class="notes">
    <div v-for="(note, index) in notes" :key="note">
    <span class="note" :class="[isSelectedItem(index) ? 'selected' : '']" @click="selectNote(index)">
      {{ getNoteHeader(index) }}
    </span>
    </div>
    <span v-if="!isAdding" @click="prepareAdding" class="add">+</span>
  </div>
  <textarea v-if="isAdding || isNoteSelected()" v-model="tempNote"></textarea>
  <div v-if="isAdding">
    <div @click="addNote" class="button">追加</div>
    <div @click="cancelAdding" class="button red">取消</div>
  </div>
  <div v-if="isNoteSelected()">
    <div @click="updateNote" class="button">編集</div>
    <div @click="deleteNote" class="button red">削除</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isAdding: false,
      selectedNoteIndex: null,
      notes: [],
      tempNote: ''
    }
  },
  watch: {
    notes: {
      handler: function () {
        localStorage.setItem('notes', JSON.stringify(this.notes))
      },
      deep: true
    }
  },
  methods: {
    getNoteHeader (index) {
      const header = this.notes[index].split("\n")[0]
      const headerLengthLimit = 20
      return header.length > headerLengthLimit ? header.slice(0, headerLengthLimit) + '...' : header
    },
    prepareAdding () {
      this.tempNote = ''
      this.selectedNoteIndex = null
      this.isAdding = true
    },
    addNote () {
      if (!this.tempNote) {
        return
      }
      this.notes.push(this.tempNote)
      this.tempNote = ''
    },
    cancelAdding () {
      this.tempNote = ''
      this.isAdding = false
    },
    selectNote (index) {
      this.isAdding = false
      this.selectedNoteIndex = index
      this.tempNote = this.notes[index]
    },
    isSelectedItem (index) {
      return index === this.selectedNoteIndex
    },
    updateNote () {
      if (!this.tempNote) {
        return
      }
      this.notes[this.selectedNoteIndex] = this.tempNote
    },
    deleteNote () {
      this.notes.splice(this.selectedNoteIndex, 1)
      this.tempNote = ''
      this.selectedNoteIndex = null
    },
    isNoteSelected () {
      return this.selectedNoteIndex !== null
    }
  },
  mounted: function () {
    this.notes = JSON.parse(localStorage.getItem('notes')) || []
  }
}
</script>

<style>
  .notes {
    text-align: left;
    margin-left: 20px;
  }
  .note {
    display: inline-block;
    margin: 20px 0 0 0;
    padding: 8px 20px;
    background: #eee;
    border-radius: 20px;
    font-size: 20px;
    letter-spacing: 2px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
  }
  .add {
    font-size: 30px;
    display: inline-block;
    margin: 20px 0 0 10px ;
    cursor: pointer;
  }
  .button {
    display: inline-block;
    margin-right: 20px;
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
    cursor: pointer;
  }
  .red {
    background: #ff0062;
  }
  .selected {
    color: #fff;
    background: #0faf87;
  }
  textarea {
    font-size: 20px;
    margin: 30px 0 0 20px;
    height: 100px;
    width: 80%;
  }
</style>
