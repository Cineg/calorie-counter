<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Montserrat');
 * {
   box-sizing: border-box;
   margin: 0; padding: 0;
 }

  #app{
    margin: 0 auto;
    font-family: 'Montserrat', sans-serif;
    color: #2c3e50;
  }
  body{
    overflow: hidden;
  }

</style>
