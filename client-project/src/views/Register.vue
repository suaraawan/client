<template>
  <div class="home">
    <div class="container">

      <Register/>
    </div>
  </div>
</template>

<script>
import Register from '@/components/Register.vue'

export default {
  name: 'register',
  components: {
    Register
  }
}
</script>

<style scoped>
.home{
  background-image: url("http://wallarthd.com/wp-content/uploads/2014/12/Cute-White-Earphones-Wallpaper-Android.jpg");
  background-size: cover;
}
</style>
