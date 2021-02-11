<template>
  <div id="app">
    <DatabaseList @changing-db="selectedDB = $event" @update-creds="creds = $event"/>
    <PartitionList :db="selectedDB" :creds="creds"/>
  </div>
</template>

<script>
import DatabaseList from './components/DatabaseList.vue'
import PartitionList from './components/PartitionList.vue'

export default {
  name: 'App',
  components: {
    DatabaseList,
    PartitionList,
  },
  
  data() {
    return {
      selectedDB: null,
      creds: null,
    }
  }
}
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
