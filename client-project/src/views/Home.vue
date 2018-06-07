<template>
  <div class="home">
    <Header/>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue'
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'home',
  components: {
    HelloWorld,
    Header
  }
}
</script>
<style scoped>
.home{
  background-image: url("https://images7.alphacoders.com/319/thumb-1920-319933.jpg");
  background-size: cover;
}
</style>
