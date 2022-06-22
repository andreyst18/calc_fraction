<template>
  <section class="section calculator">
    <div class="calculator__input-fractions input-fractions">
      <Fraction @add-fraction="addFraction"></Fraction>
      <PlusSymbol class="input-fractions__plus"></PlusSymbol>
      <Fraction @add-fraction="addFraction"></Fraction>
      <EqualSymbol class="input-fractions__equal"></EqualSymbol>
      <div class="input-fractions__result result">
        <div class="result__whole">{{ getResult }}</div>
        <div class="result__fractional fractional">
          <div class="fractional__item"></div>
          <div class="fractional__item"></div>
        </div>
      </div>
    </div>
    <!-- <AddFractionBtn></AddFractionBtn>
    <SaveCalcBtn></SaveCalcBtn> -->
  </section>
</template>

<script>
import Fraction from './Fraction.vue'
import PlusSymbol from './PlusSymbol.vue'
import EqualSymbol from './EqualSymbol.vue'

export default {
  components: {
    Fraction,
    PlusSymbol,
    EqualSymbol
  },
  data() {
    return {
      fractions: [],
      result: '?',
      fractionsQuantity: 2
    };
  },
  methods: {
    addFraction(numerator, denominator) {
      this.fractions.push([+numerator, +denominator]);
      console.log(this.fractions);

      if(this.checkResultConditions) {
        console.log(this.fractions[0]);
        this.getResult;
      }
      
    },
    checkResultConditions() {
      if (this.fractions.length === this.fractionsQuantity) {
        return true;
      }
      return false;
    }
    
  },
  computed: {
    getResult() {
      if (!this.checkResultConditions()) {
        return  '?';
      }
      let result = 0;
      this.fractions.forEach( el => {
        result += el[1]
      });
      return result;
    }
  }
};
</script>

<style lang="scss" scoped>
.input-fractions {
  display: flex;
  &__plus,
  &__equal,
  &__result {
    padding: 15px;
  }
}

.result {
  display: flex;
}
</style>
