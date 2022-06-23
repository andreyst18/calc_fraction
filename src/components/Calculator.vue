<template>
  <section class="section calculator">
    <div class="calculator__input-fractions input-fractions">
      <Fraction @add-fraction="addFraction"></Fraction>
      <PlusSymbol class="input-fractions__plus"></PlusSymbol>
      <Fraction @add-fraction="addFraction"></Fraction>
      <EqualSymbol class="input-fractions__equal"></EqualSymbol>
      <div class="input-fractions__result result">
        <div class="result__whole">{{ getWholePartResult[0] }}</div>
        <div class="result__fractional fractional">
          <div class="fractional__item">{{ getWholePartResult[1] }}</div>
          <div class="fractional__item"></div>
        </div>
      </div>
    </div>
    <!-- <AddFractionBtn></AddFractionBtn>
    <SaveCalcBtn></SaveCalcBtn> -->
  </section>
</template>

<script>
import Fraction from "./Fraction.vue";
import PlusSymbol from "./PlusSymbol.vue";
import EqualSymbol from "./EqualSymbol.vue";

export default {
  components: {
    Fraction,
    PlusSymbol,
    EqualSymbol,
  },
  data() {
    return {
      fractions: [],
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
  },
  computed: {
    getWholePartResult() {
      if (!this.checkResultConditions()) {
        return "?";
      }

      const commonMultiple = this.getCommonMultiple;
      const fractionsModified = [];
      let sum = 0;
      let wholeResult = 0;
      let numeratorResult = 0;
      let denominatorResult = 0;
      for (let i = 0; i < this.fractions.length; i++) {
        const multiplier = commonMultiple / this.fractions[i][1];
        const fractionModified = this.fractions[i].map(
          (el) => (el *= multiplier)
        );
        fractionsModified.push(fractionModified);
        sum += fractionModified[0];
      }
      wholeResult = Math.floor(sum / fractionsModified[0][1]);
      //здесь будет расчет - числитель и знаменатель
      return [wholeResult, sum % fractionsModified[0][1]];
    },
    // getNumeratorResult() {

    // },
    // getDenominatorResult() {

    // },
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
