<template>
  <div id="app">
    <div class="content">
      <h1>Форма создания клиента</h1>
      <Createform />
    </div>
  </div>
</template>

<script>
import Createform from './components/Createform.vue'

export default {
  name: 'App',
  components: {
    Createform
  }
}
</script>

<style lang="scss">
  @import 'assets/index.scss';
</style>
