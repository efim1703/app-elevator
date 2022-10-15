<template>
  <div id="app">
    <elevator-application :settingsForApplication="settingsForApplication"/>
  </div>
</template>

<script>
import ElevatorApplication from './components/ElevatorApplication/Index.vue'

export default {
  name: 'App',
  data() {
    return {
      settingsForApplication: {
        numberOfFloors: 6
      }
    }
  },
  components: {
    ElevatorApplication
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

</style>
