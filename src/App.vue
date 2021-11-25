<template>
  <Notes />
</template>

<script>
import Notes from '@/components/Notes'
export default {
  name: "App",
  components: { Notes },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding-left: 40px;
}
</style>
