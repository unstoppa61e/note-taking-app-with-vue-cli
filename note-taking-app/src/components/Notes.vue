<template>
  <div v-for="(note, index) in notes" :key="note">
    <span @click="focusNote(index)">
      {{ note }}
    </span>
    <span @click="isAdding = true;console.log(isAdding)">+</span>
    <textarea v-if="isAdding"></textarea>
  </div>

</template>

<script>
export default {
  data() {
    return {
      isAdding: false,
      isFocusing: false,
      notes: []
    }
  },
  watch: {
    notes: {
      handler: function () {
        localStorage.setItem('notes', JSON.stringify(this.notes))
      }
    },
    deep: true
  },
  methods: {
    focusNote () {

    },
  }
}
</script>

<style scoped>

</style>
