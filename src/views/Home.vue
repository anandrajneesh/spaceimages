<template>
  <div class="banner d-flex">
    <div class="col-4"></div>
    <div class="col-4 text-center align-self-center">
        <div>
          <h1 class="display-2" style="color: white"> Nasa Image Search</h1>
          <Search @search="search" :text="undefined"/>
        </div>
    </div>
  </div>
</template>

<script>
import Search from '../components/Search.vue'

export default {
  name: 'Home',
  components: {
    Search
  },
  emits : ['search'],
  methods: {
    search(value){
      this.$emit('search', value)
    }
  }
}
</script>
<style scoped>
.banner {
  background-image: url("../assets/home_banner.jpeg");
  min-height: 100%;
}
</style>
