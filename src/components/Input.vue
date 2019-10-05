<template lang="html">
<div class="mt-5">
  <select class="custom-select" v-model="selectedBit">
    <option v-for="i in maxBit - 1" :value="i+1">{{ i+1 }}</option>
  </select>

  <h2 class="mt-5 ml-2">Which minterm is on?</h2>
  <div class="row mt-5 text-center">
    <div class="col-4 mt-2" v-for="i in 2**selectedBit">
      <button :class="`btn btn-outline-dark btn-lg ${minTermActive[i-1]?'active':''}`" :key="`m${i-1}`" @click="onMinTermClick(i-1)">m{{ i-1 }}</button>
    </div>
  </div>

  <div class="text-center mt-5">
    <button class="btn btn-primary" @click="$emit('completeInput',{ selectedMinTerm, selectedBit })">Calculate</button>
  </div>

</div>
</template>

<script>
export default {
  data() {
    return {
      selectedBit: 2,
      maxBit: 4,
      minTermActive: [],
      selectedMinTerm: []
    }
  },
  created() {
    this.minTermActive = [...Array(2**this.maxBit).keys()].map(() => false)
  },
  watch: {
    minTermActive(minTerms) {
      this.selectedMinTerm = minTerms.map((isActive, index) => ({isActive, index})).filter((minTerm) => minTerm.isActive == true).map((minTerm) => minTerm.index)
    }
  },
  methods: {
    onMinTermClick(m) {
      this.minTermActive.splice(m,1,!this.minTermActive[m])
    }
  }
}
</script>

<style lang="css" scoped>
</style>
