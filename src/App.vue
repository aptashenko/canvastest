<template>
  <PainComp />
</template>

<script>
import PainComp from './components/PainComp.vue'

export default {
  components: { PainComp }
}
</script>

<style>
* {
  box-sizing: border-box;
}

html, body {
  margin: 0;
}

#app {
  display: flex;
}
</style>
