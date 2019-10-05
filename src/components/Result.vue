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
      // groups: [],
      pairIndex: []
    }
  },
  created() {
    // this.groups = [...Array(this.selectedBit+1).keys()].map(() => [])
  },
  mounted() {
    // console.log('Calculation Start');
    this.createInitialTable(this.minTerms)
    // this.checkConcat(this.initialTable)
    // this.groupMinTerm(this.initialTable)
    // console.log(x);
    for(let i=0;i<3;i++) {
      let x = this.checkConcat(this.initialTable)
      console.log(x);
      this.initialTable = this.groupMinTerm(this.initialTable)
      // console.log(this.pairIndex.length);
    }
    console.log(this.initialTable);
  },
  methods: {
    createInitialTable(minTerms) {
      this.initialTable = minTerms.map((minTerm) => {
                    const g = minTerm.toString(2).split('').reduce((sum, bin) => sum+(bin - '0'), 0)
                    // this.groups[g].push(minTerm)
                    let bin = minTerm.toString(2).split('')
                    bin = [...Array(this.selectedBit-bin.length).keys()].map(() => '0').concat(...bin)
                    return {
                      group: g,
                      minTerm,
                      bin,
                      concated: [minTerm]
                    }
                  })
      // console.log(this.groups);
    },
    checkConcat(table) {
      // table.sort((a, b) => a.group<b.group)
      for(let i=0; i<table.length; i++) {
        let currentMinTerm = table[i]
        for(let j=i+1; j<table.length; j++) {
          let comparedMinTerm = table[j]
          if(comparedMinTerm.group - currentMinTerm.group == 1) {
            let binA = currentMinTerm.bin
            let binB = comparedMinTerm.bin

            // console.log(binA,binB);
            let differentBit = binA.filter((b, index) => b!=binB[index])
            console.log(binA + ' [COMPARE] ' + binB);
            console.log(differentBit.length);
            if(differentBit.length == 1) {
              let resultBin = binA.reduce((newBin, b, index) => {
                            if(b!=binB[index]) {
                              return newBin + '-'
                            }
                            return newBin + b
                          },'')
              console.log('[RESULT] ' + resultBin);

              this.pairIndex.push({i, j, bin: resultBin.split('')})
            }
          }
        }
      }
      return this.pairIndex.length > 0
    },
    groupMinTerm(table) {
      let deletedIndex = []
      while(this.pairIndex.length > 0) {
        let pair = this.pairIndex.shift()
        if(!deletedIndex.includes(pair.i)) deletedIndex.push(pair.i)
        if(!deletedIndex.includes(pair.j)) deletedIndex.push(pair.j)
        let clone = table[pair.i]
        clone.bin = pair.bin
        table.push(clone)
      }

      // console.log(deletedIndex);
      deletedIndex.forEach((i) => {
        table.splice(i,1)
        // console.log('[test]',bf,table.length);
      })
      // console.log(table.length);
      return table
    }

  }
}
</script>

<style lang="css" scoped>
</style>
