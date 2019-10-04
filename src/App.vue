<template>
  <div class="container mt-5">
    <h1 class="text-center">Quine McCluskey</h1>
    <Input v-if="!isInputComplete" @completeInput="value => prepareCalculation(value)"/>
    <Result v-else :minTerms="selectedMinTerm" :selectedBit="selectedBit" @reInput="isInputComplete = false"/>
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
      selectedMinTerm: [],
      isInputComplete: false,
      selectedBit: 1
    }
  },
  methods: {
    prepareCalculation(value) {
      this.selectedMinTerm = value.selectedMinTerm
      this.selectedBit = value.selectedBit
      this.isInputComplete = true
    }
  }
}
</script>

<style>
</style>
