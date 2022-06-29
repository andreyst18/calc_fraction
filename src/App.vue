<template>
  <div id="app" class="wrapper">
    <section class="section calculator">
      <h2 class="calculator__title">Калькулятор</h2>
      <div class="calculator__input-fractions input-fractions">
        <ul class="calculator__additional-fractions additional-fractions">
          <li
            class="additional-fractions__item"
            v-for="(item, index) in fractions"
            :key="item.id"
          >
            <Fraction
              @add-fraction="addFraction"
              :isFirst="index === 0"
              :index="index"
              :id="item.id"
            >
              <CrossBtn
                v-if="fractionsQuantity > 2"
                @delete-fraction="deleteFraction(index)"
              ></CrossBtn>
            </Fraction>
          </li>
        </ul>
        <EqualSymbol class="input-fractions__equal"></EqualSymbol>
        <div class="input-fractions__result result">
          <div class="result__whole">{{ getResult[0] || "" }}</div>
          <div class="result__fractional fractional">
            <div class="fractional__item">{{ getResult[1] || "" }}</div>
            <div class="fractional__item">{{ getResult[2] || "" }}</div>
          </div>
        </div>
      </div>
      <AddFractionBtn @add-fraction="addNewFractionForm()"></AddFractionBtn
      ><br />
      <SaveCalcBtn @save-calc="saveCalculation"></SaveCalcBtn>
    </section>
    <section class="calculations">
      <h2 class="calculations__title">Расчеты</h2>
      <ul class="calculations__list list">
        <li
          class="list__item"
          v-for="(itemOuter, indexOuter) in savedFractions"
          :key="indexOuter"
        >
          <ul class="expression">
            <li
              class="expression__item"
              v-for="(itemInner, indexInner) in savedFractions[indexOuter]"
              :key="indexInner"
            >
              {{ itemInner.numerator / itemInner.denominator }}
            </li>
          </ul>
          <div class="expression__result">{{ savedResults[indexOuter] }}</div>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
import Fraction from "./components/Fraction.vue";
import EqualSymbol from "./components/EqualSymbol.vue";
import AddFractionBtn from "./components/AddFractionBtn.vue";
import CrossBtn from "./components/CrossBtn.vue";
import SaveCalcBtn from "./components/SaveCalcBtn.vue";

export default {
  name: "App",
  components: {
    Fraction,
    EqualSymbol,
    AddFractionBtn,
    CrossBtn,
    SaveCalcBtn,
  },
  data() {
    return {
      fractions: [
        {
          id: 0,
          numerator: "",
          denominator: "",
        },
        {
          id: 1,
          numerator: "",
          denominator: "",
        },
      ],
      result: "?",
      fractionsQuantity: 2,
      lastID: 1,
      savedFractions: JSON.parse(localStorage.getItem("fractions")) || [],
      savedResults: JSON.parse(localStorage.getItem("results")) || [],
    };
  },
  methods: {
    addFraction(numerator, denominator, index, id) {
      this.fractions.splice(index, 1, {
        id: id,
        numerator: +numerator,
        denominator: +denominator,
      });

      if (this.checkResultConditions) {
        this.getResult;
      }
    },
    checkResultConditions() {
      if (
        this.fractions.every(
          (el) =>
            el.numerator === +el.numerator && el.denominator === +el.denominator
        )
      ) {
        return true;
      }
      return false;
    },
    addNewFractionForm() {
      if (this.fractionsQuantity < 5) {
        this.fractionsQuantity++;
        this.fractions.push({
          id: ++this.lastID,
          numerator: "",
          denominator: "",
        });
      }
    },
    deleteFraction(index) {
      this.fractions.splice(index, 1);
      this.fractionsQuantity--;
    },
    saveCalculation() {
      console.log(this.fractions);
      this.savedFractions.push(this.fractions);
      this.savedResults.push([
        this.getResult[0],
        this.getResult[1],
        this.getResult[2],
      ]);

      const serializedFractions = JSON.stringify(this.savedFractions);
      const serializedResults = JSON.stringify(this.savedResults);
      localStorage.setItem("fractions", serializedFractions);
      localStorage.setItem("results", serializedResults);

      ++this.lastID;
      this.fractions = [
        {
          id: this.lastID,
          numerator: "",
          denominator: "",
        },
        {
          id: ++this.lastID,
          numerator: "",
          denominator: "",
        },
      ];
    },
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

      return [
        wholeResult,
        numeratorResult / greatestCommonDivisor,
        denominatorResult / greatestCommonDivisor,
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

.calculator {
  &__additional-fractions {
    display: flex;
  }
}

.additional-fractions {
  margin: 0;
  &__item {
    display: flex;
  }
}
</style>
