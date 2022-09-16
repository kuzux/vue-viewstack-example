<script>
import Sidebar from './Sidebar.vue'
import MainView from './MainView.vue'

export default {
  components: { Sidebar, MainView },
  methods: {
    onMainSelect(item) {
      this.$refs.sidebar.goTo(item)
    }
  }
}
</script>

<template>
  <header>
    <Sidebar ref="sidebar" />
  </header>
  <main>
    <MainView @selected="onMainSelect" />
  </main>
</template>

<style scoped>
  header {
    margin-right: 2em;
  }
</style>
