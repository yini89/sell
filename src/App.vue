<template>
  <div id="app">
    <!--<div class="header">-->
      <header></header>
    <!--</div>-->
    <div class="tab">
      I am tab!
    </div>
    <div class="content">
      I am content!
    </div>
    <!--<router-view/>-->
  </div>
</template>

<script>
  import header from './components/header/header';
  export default {
    components: {
      'header': header
    }
  };
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
}
</style>
