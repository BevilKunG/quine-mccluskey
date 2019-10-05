<template lang="html">
<div>
  <!-- {{ minTerms }} -->
  <h1 class="text-center mt-5">{{ result }}</h1>
  <div class="mt-5">
    <h4>1-minterm: <span v-for="minTerm in minTerms">{{ minTerm + ' ' }}</span></h4>
  </div>
</div>
</template>

<script>
export default {
  props:['minTerms','selectedBit'],
  data() {
    return {
      initialTable: [],
      pairIndex: [],
      result: ''
    }
  },
  created() {
  },
  mounted() {
    this.createInitialTable(this.minTerms)
    for(let i=0;i<100 ;i++) {
      // console.log('[Round] ' + (i+1));
      let canConcat = this.checkConcat(this.initialTable)
      if(!canConcat) break
      // console.log(canConcat);
      this.initialTable = this.groupMinTerm(this.initialTable)
      // console.log(this.initialTable.map((item) => item.bin.join('')));
      // console.log(this.initialTable.map((item) => item.concated));


    }
    this.result = this.calculateResult(this.initialTable.map((item) => item.bin))
  },
  methods: {
    createInitialTable(minTerms) {
      this.initialTable = minTerms.map((minTerm) => {
                    const g = minTerm.toString(2).split('').reduce((sum, bin) => sum+(bin - '0'), 0)
                    let bin = minTerm.toString(2).split('')
                    bin = [...Array(this.selectedBit-bin.length).keys()].map(() => '0').concat(...bin)
                    return {
                      group: g,
                      minTerm,
                      bin,
                      concated: [minTerm]
                    }
                  })
    },
    checkConcat(table) {
      for(let i=0; i<table.length; i++) {
        let currentMinTerm = table[i]
        for(let j=i+1; j<table.length; j++) {
          let comparedMinTerm = table[j]

          if(comparedMinTerm.group - currentMinTerm.group == 1) {
            let binA = currentMinTerm.bin
            let binB = comparedMinTerm.bin

            // console.log(binA,binB);
            let differentBit = binA.filter((b, index) => b!=binB[index])
            if(differentBit.length == 1) {
              // console.log(binA + ' [COMPARE] ' + binB);
              let resultBin = binA.reduce((newBin, b, index) => {
                            if(b!=binB[index]) {
                              return newBin + '-'
                            }
                            return newBin + b
                          },'')
              // console.log('[RESULT] ' + resultBin);

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
        let clone = {...table[pair.i]}
        // console.log('['+ pair.bin+ '] '+table[pair.i].minTerm + ' ' + table[pair.j].minTerm);
          if(!deletedIndex.includes(pair.i)) deletedIndex.push(pair.i)
          if(!deletedIndex.includes(pair.j)) deletedIndex.push(pair.j)
          clone.bin = pair.bin
          table.push(clone)
      }
      // console.log(deletedIndex);
      deletedIndex.sort((a, b) => b-a).forEach((i) => {
        // console.log(table[i].bin);
        table.splice(i,1)
      })
      return table.sort((a,b) => a.group-b.group)
    },
    calculateResult(resultBinary) {
      // console.log(resultBinary.map(bin => bin.join('')).filter((value,index, self) => self.indexOf(value) === index));
      return resultBinary.map(bin => bin.join(''))
              .filter((value,index, self) => self.indexOf(value) === index)
              .map(bin => bin.split(''))
              .map(bin => {
                return bin.map((b, index) => {
                  let term = String.fromCharCode('A'.charCodeAt(0) + index)
                  if(b=='-') return ''
                  return term + (b=='1'?'':'\'')
                })
                .join('')
              })
              .map( (term, index, self) => {
                return term + (index < self.length - 1?' + ':'')
              })
              .join('')
    }

  }
}
</script>

<style lang="css" scoped>
</style>
