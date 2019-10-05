<template>
  <div class="container mt-5">
    <button v-if="isInputComplete" class="btn btn-primary btn-sm" @click="isInputComplete = false">back</button>
    <h1 class="text-center">Quine McCluskey</h1>
    <Input v-if="!isInputComplete" @completeInput="value => prepareCalculation(value)"/>
    <Result v-else :minTerms="selectedMinTerm" :selectedBit="selectedBit"/>
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
