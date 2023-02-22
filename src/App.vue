<template>
  <div id="app">
    <Calendar />
  </div>
</template>

<script>
import Calendar from '@/views/Calendar.vue'

export default {
components: { Calendar }

}

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
