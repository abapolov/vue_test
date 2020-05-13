<template>
  <div id="app">
    <Search></Search>
  </div>
</template>

<script>
import Search from './global/components/Search.vue'

export default {
  name: 'App',
  components: {
    Search
  }
}
</script>

<style>
  @import "./global/styles/global.scss";
</style>
