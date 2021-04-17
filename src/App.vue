<template>
  <div id="app">
    <CustomSelect :options="options" v-model="selected"/>
  </div>
</template>

<script>
import CustomSelect from './components/CustomSelect.vue'

export default {
  name: 'App',
  components: {
    CustomSelect
  },
  data() {
    return {
      selected: [],
      options: [
        {
          value: 'option',
          label: 'Кукурузные Хлопья'
        },
        {
          value: 'option2',
          label: 'Лавровый листик'
        },
        {
          value: 'option3',
          label: 'Ванильный сахарок'
        },
        {
          value: 'option4',
          label: 'Овсяное молочко'
        }]
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
