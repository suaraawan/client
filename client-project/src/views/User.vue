<template>
  <div class="home">
      <Header/>
      <User/>
  </div>
</template>

<script>
import User from '@/components/User.vue'
import Header from '@/components/Header.vue'

export default {
  name: 'music',
  components: {
    User,
    Header
  }
}
</script>

<style scoped>
</style>
