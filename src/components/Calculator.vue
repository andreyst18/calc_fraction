<template>
  <section class="section calculator">
    <div class="calculator__input-fractions input-fractions">
      <ul class="calculator__additional-fractions additional-fractions">
        <li class="additional-fractions__item" 
            v-for="(item, index) in fractions" 
            :key="item.id"
        >
          <Fraction @add-fraction="addFraction" 
                    :isFirst="index === 0" 
                    :index="index"
                    :id="item.id"
          >
            <CrossBtn v-if="fractionsQuantity > 2" @delete-fraction="deleteFraction(index)"></CrossBtn>
          </Fraction>
        </li>
      </ul>
      <EqualSymbol class="input-fractions__equal"></EqualSymbol>
      <div class="input-fractions__result result">
        <div class="result__whole">{{ getResult[0] || '' }}</div>
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
import EqualSymbol from "./EqualSymbol.vue";
import AddFractionBtn from "./AddFractionBtn.vue";
import CrossBtn from "./CrossBtn.vue";

export default {
  components: {
    Fraction,
    EqualSymbol,
    AddFractionBtn,
    CrossBtn
  },
  data() {
    return {
      fractions: [
        {
          id: 0,
          numerator: '',
          denominator: ''
        }
        , 
        {
          id: 1,
          numerator: '',
          denominator: ''
        }
        ,
      ],
      result: "?",
      fractionsQuantity: 2,
      lastID: 1
    };
  },
  methods: {
    addFraction(numerator, denominator, index, id) {
      console.log(id)
      console.log(index)
      console.log(numerator)
      this.fractions.splice(index, 1, {
        id: id,
        numerator: +numerator,
        denominator: +denominator
      });
      console.log(this.fractions)

      if (this.checkResultConditions) {
        this.getResult;
      }
    },
    checkResultConditions() {
      if (this.fractions.every( el => el.numerator === +el.numerator &&
                                      el.denominator === +el.denominator)) {
        return true;
      }
      return false
    },
    addNewFractionForm() {
      if (this.fractionsQuantity < 5) {
        this.fractionsQuantity++;
        console.log(this.fractionsQuantity)
        this.fractions.push({
          id: ++this.lastID,
          numerator: '',
          denominator: ''
        });
      }
      console.log(this.fractions)
    },
    deleteFraction(index) {
      console.log(index)
      this.fractions.splice(index, 1);
      this.fractionsQuantity--;
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
        const multiplier = commonMultiple / this.fractions[i].denominator;
        const fractionModified = {
          numerator: this.fractions[i].numerator * multiplier,
          denominator: this.fractions[i].denominator * multiplier,
        };
        fractionsModified.push(fractionModified);
        sum += fractionModified.numerator;
      }
      let wholeResult = Math.floor(sum / fractionsModified[0].denominator);
      let numeratorResult = sum % fractionsModified[0].denominator;
      let denominatorResult = fractionsModified[0].denominator;

      //Попытка сократить дробь
      let greatestCommonDivisor = 1;
      for (let i = 2; i <= numeratorResult; i++) {
        if (numeratorResult % i === 0 && denominatorResult % i === 0) {
          greatestCommonDivisor = i;
        }
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
        result *= el.denominator;
      });
      return result;
    },
    
  },
};
</script>

<style lang="scss" scoped>
* {
  margin: 0;
  padding: 0;
}

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

.calculator{
  &__additional-fractions {
    display: flex;
  }
}

.additional-fractions{
  margin: 0;
  &__item {
    display: flex;
    
  }
}
</style>
