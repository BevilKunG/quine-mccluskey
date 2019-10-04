<template lang="html">
<div>
  <button class="btn btn-primary btn-sm" @click="$emit('reInput')">back</button>
  {{ minTerms }}
</div>
</template>

<script>
export default {
  props:['minTerms','selectedBit'],
  data() {
    return {
      initialTable: [],
      groups: []
    }
  },
  created() {
    this.groups = [...Array(this.selectedBit+1).keys()].map(() => [])
  },
  mounted() {
    // console.log('Calculation Start');
    this.createInitialTable(this.minTerms)
  },
  methods: {
    createInitialTable(minTerms) {
      this.initialTable = minTerms.map((minTerm) => {
                    const g = minTerm.toString(2).split('').reduce((sum, bin) => sum+(bin - '0'), 0)
                    this.groups[g].push(minTerm)
                    return { group: g, minTerm }
                  })
      // console.log(this.groups);
    },
  }
}
</script>

<style lang="css" scoped>
</style>
