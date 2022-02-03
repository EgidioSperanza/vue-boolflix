<template>
  <header>
    <h1>Boolflix</h1>
    <div class="search">
      <input type="text" v-model="stringToSearch"/>
      <button @click="$emit('searchByTitle', stringToSearch)">Search</button>
    </div>
  </header>
</template>

<script>
export default {
  name: 'HeaderApp',
  data(){
    return{
      stringToSearch:''
    }
  },
  props: {},
}
</script>

<style scoped lang="scss">
@import '@/style/variables.scss';
h1 {
  color: $title-color;
  text-transform: uppercase;
  font-size: 50px;
}
input {
  height: 50px;
  width: 200px;
}
button {
  height: 50px;
  margin-left:20px;
  padding: 0 20px;
}
</style>
