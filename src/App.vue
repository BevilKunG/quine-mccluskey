<template>
  <div class="container mt-5">
    <h1 class="text-center">Quine McCluskey</h1>
    <Input />
    <!-- <Result v-else/> -->
  </div>
</template>

<script>
import Input from './components/Input.vue'
import Result from './components/Result.vue'

export default {
  name: 'app',
  components: {
    Input,
    Result
  },
  data() {
    return {
    }
  }
}
</script>

<style>
</style>
