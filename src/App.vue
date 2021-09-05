<template>
  <router-view @search="search" :key="$route.path"/>
</template>

<script>
export default {
  name : 'App',
  methods:{
    search(value){
        console.log(`In App.vue ${value}`)
        this.$router.push({name : 'Results', params : {search :value}})
    }
  }
}
</script>

<style>
body{
  display: flex;
  height: 100vh;
  width: 100vw;
  margin: 0;
}
</style>
