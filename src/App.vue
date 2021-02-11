<template>
  <div id="app">
    <FormCredidentials/>
    <DatabaseList @changing-db="selectedDB = $event"/>
    <PartitionList :db="selectedDB"/>
  </div>
</template>

<script>
import DatabaseList from './components/DatabaseList.vue'
import PartitionList from './components/PartitionList.vue'
import FormCredidentials from './components/FormCredidentials.vue'

export default {
  name: 'App',
  components: {
    DatabaseList,
    PartitionList,
    FormCredidentials,
  },
  
  data() {
    return {
      selectedDB: null,
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
