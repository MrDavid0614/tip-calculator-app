<template>
  <div class="main-container">
    <Home />
    <Footer />
  </div>
</template>

<script>
import Home from './views/Home.vue'
import Footer from './components/Footer.vue'

export default {
  components: { 
    Home,
    Footer,
  }
}
</script>

<style scoped>

.main-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

</style>
