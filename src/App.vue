<template>
  <div id="app">
  
      <leftNav></leftNav>
      <Pos></Pos>
      <router-view></router-view>
  </div>
</template>

<script>
import leftNav from './components/leftNav'
import Pos from './components/pos'
export default {
  name: 'app',
  components : {
    leftNav,Pos
  },
  created(){
    let oHeight=document.body.clientHeight;
    document.getElementById("app").style.height= oHeight + "px";

  }


}
</script>

<style>

*{ margin:0; padding:0; }

html,body,#app{ height:100%;
           background:#f1f1f1; }

li{ list-style:none; }

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}
</style>
