<template>
  <section class="section calculator">
    <div class="calculator__input-fractions input-fractions">
      <Fraction @add-fraction="addFraction"></Fraction>
      <PlusSymbol class="input-fractions__plus"></PlusSymbol>
      <Fraction @add-fraction="addFraction"></Fraction>
      <ul>
        <li v-for="(item, index) in additionalFractions" :key="index">
          <PlusSymbol class="input-fractions__plus"></PlusSymbol>
          <Fraction @add-fraction="addFraction"></Fraction>
        </li>
      </ul>
      <EqualSymbol class="input-fractions__equal"></EqualSymbol>
      <div class="input-fractions__result result">
        <div class="result__whole">{{ getResult[0] }}</div>
        <div class="result__fractional fractional">
          <div class="fractional__item">{{ getResult[1] || '' }}</div>
          <div class="fractional__item">{{ getResult[2] || '' }}</div>
        </div>
      </div>
    </div>
    <AddFractionBtn @add-fraction="addNewFractionForm()"></AddFractionBtn>
    <!-- <SaveCalcBtn></SaveCalcBtn> -->
  </section>
</template>

<script>
import Fraction from "./Fraction.vue";
import PlusSymbol from "./PlusSymbol.vue";
import EqualSymbol from "./EqualSymbol.vue";
import AddFractionBtn from "./AddFractionBtn.vue"

export default {
  components: {
    Fraction,
    PlusSymbol,
    EqualSymbol,
    AddFractionBtn
  },
  data() {
    return {
      fractions: [],
      additionalFractions: [],
      result: "?",
      fractionsQuantity: 2,
    };
  },
  methods: {
    addFraction(numerator, denominator) {
      this.fractions.push([+numerator, +denominator]);

      if (this.checkResultConditions) {
        this.getResult;
      }
    },
    checkResultConditions() {
      if (this.fractions.length === this.fractionsQuantity) {
        return true;
      }
      return false;
    },
    addNewFractionForm() {
      if (this.fractionsQuantity < 5) {
        this.additionalFractions.push(1);
        this.fractionsQuantity++
      }
      console.log(this.fractions)
    }
  },
  computed: {
    getResult() {
      if (!this.checkResultConditions()) {
        return "?";
      }

      const commonMultiple = this.getCommonMultiple;
      const fractionsModified = [];
      let sum = 0;
      for (let i = 0; i < this.fractions.length; i++) {
        const multiplier = commonMultiple / this.fractions[i][1];
        const fractionModified = this.fractions[i].map(
          (el) => (el *= multiplier)
        );
        fractionsModified.push(fractionModified);
        sum += fractionModified[0];
      }
      let wholeResult = Math.floor(sum / fractionsModified[0][1]);
      let numeratorResult = sum % fractionsModified[0][1];
      let denominatorResult = fractionsModified[0][1];

      //Попытка сократить дробь
      let greatestCommonDivisor = 1;
      for (let i = 2; i <= numeratorResult; i++) {
        if (numeratorResult % i === 0 && denominatorResult % i === 0) {
          greatestCommonDivisor = i;
        }
        console.log(greatestCommonDivisor)
      }

      denominatorResult = numeratorResult === 0 ? 0 : denominatorResult;
      
      return [wholeResult, 
              numeratorResult / greatestCommonDivisor,
              denominatorResult / greatestCommonDivisor
            ];
    },
    getCommonMultiple() {
      let result = 1;
      this.fractions.forEach((el) => {
        result *= el[1];
      });
      return result;
    },
    
  },
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
