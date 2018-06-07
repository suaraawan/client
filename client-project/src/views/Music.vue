<template>
  <div class="home">
    <div class="container">

      <Music/>
    </div>
  </div>
</template>

<script>
import Music from '@/components/Musicpage.vue'

export default {
  name: 'music',
  components: {
    Music
  }
}
</script>

<style scoped>
.home{
  background-image: url("http://wallarthd.com/wp-content/uploads/2014/12/Cute-White-Earphones-Wallpaper-Android.jpg");
  background-size: cover;
}
</style>
