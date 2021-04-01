<template>
  <div id="app">
    <hello-world/>
  </div>
</template>

<script>
import HelloWorld from './components/Main.vue'
import '../src/components/evreka.css';
export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>
