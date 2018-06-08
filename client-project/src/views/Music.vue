<template>
  <div class="home">
      <Header/>
      <Music/>
  </div>
</template>

<script>
import Music from '@/components/Musicpage.vue'
import Header from '@/components/Header.vue'

export default {
  name: 'music',
  components: {
    Music,
    Header
  }
}
</script>

<style scoped>
</style>
