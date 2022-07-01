<template>
  <div id="app" class="wrapper">
    <section class="calculator">
      <h2 class="calculator__title title">Калькулятор</h2>
      <div class="main">
        <div class="calculator__input-fractions input-fractions">
          <ul class="input-fractions__list">
            <li
              class="input-fractions__item"
              v-for="(item, index) in fractions"
              :key="item.id"
            >
              <Fraction
                @add-fraction="addFraction"
                :isFirst="index === 0"
                :index="index"
                :id="item.id"
                class="input-fractions__fraction"
              >
                <CrossBtn
                  v-show="fractionsQuantity > 2"
                  @delete-fraction="deleteFraction(index)"
                  class="input-fractions__cross"
                ></CrossBtn>
              </Fraction>
            </li>
          </ul>
          <EqualSymbol class="input-fractions__equal"></EqualSymbol>
          <div class="input-fractions__result result">
            <div class="result__whole">{{ getResult[0] || "" }}</div>
            <div class="result__fractional fractional">
              <div class="fractional__item">
                <div>{{ getResult[1] || "" }}</div>
              </div>
              <div class="line" v-if="getResult[2]"></div>
              <div class="fractional__item">
                <div>{{ getResult[2] || "" }}</div>
              </div>
            </div>
          </div>
        </div>
        <AddFractionBtn
          class="calculator__new-fraction-btn"
          @add-fraction="addNewFractionForm()"
        ></AddFractionBtn
        ><br />
        <SaveCalcBtn @save-calc="saveCalculation"></SaveCalcBtn>
      </div>
    </section>
    <section class="calculations">
      <h2 class="calculations__title title">Расчеты</h2>
      <div class="main">
        <div
          class="calculations__default-text"
          v-if="!this.savedFractions.length > 0"
        >
          Тут будут расчеты
        </div>
        <ul class="calculations__list list">
          <li
            class="list__item"
            v-for="(itemOuter, indexOuter) in savedFractions"
            :key="itemOuter.id"
          >
            <div class="calculations__expression">
              <ul class="expression list">
                <li
                  class="expression__item"
                  v-for="(itemInner, indexInner) in savedFractions[indexOuter]
                    .fractions"
                  :key="indexInner"
                >
                  <PlusSymbol
                    v-if="indexInner !== 0"
                    class="expression__plus"
                  ></PlusSymbol>

                  <div class="expression__fraction">
                    <div class="expression__number fractional__item">
                      {{ itemInner.numerator }}
                    </div>
                    <div class="line"></div>
                    <div class="expression__number fractional__item">
                      {{ itemInner.denominator }}
                    </div>
                  </div>
                </li>
              </ul>
              <div class="expression__equal">
                <EqualSymbol></EqualSymbol>
              </div>
              <div class="expression__result result">
                <div
                  class="result__whole"
                  v-if="savedFractions[indexOuter].results.whole"
                >
                  {{ savedFractions[indexOuter].results.whole }}
                </div>
                <div class="result__fractional fractional">
                  <div class="fractional__item">
                    <div>
                      {{ savedFractions[indexOuter].results.numerator || "" }}
                    </div>
                  </div>
                  <div
                    class="line"
                    v-if="
                      savedFractions[indexOuter].results.numerator &&
                      savedFractions[indexOuter].results.denominator
                    "
                  ></div>
                  <div class="fractional__item">
                    <div>
                      {{ savedFractions[indexOuter].results.denominator || "" }}
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <div class="expression__delete-btn">
              <DeleteBtn
                @delete-calculation="deleteCalculation"
                :index="indexOuter"
              ></DeleteBtn>
            </div>
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
import Fraction from "./components/Fraction.vue";
import EqualSymbol from "./components/EqualSymbol.vue";
import AddFractionBtn from "./components/AddFractionBtn.vue";
import CrossBtn from "./components/CrossBtn.vue";
import SaveCalcBtn from "./components/SaveCalcBtn.vue";
import DeleteBtn from "./components/DeleteBtn.vue";
import PlusSymbol from "./components/PlusSymbol.vue";

export default {
  name: "App",
  components: {
    Fraction,
    EqualSymbol,
    AddFractionBtn,
    CrossBtn,
    SaveCalcBtn,
    DeleteBtn,
    PlusSymbol,
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
      saveID: 1,
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
      if (this.checkResultConditions()) {
        this.savedFractions.push({
          id: this.saveID,
          fractions: this.fractions,
          results: {
            whole: this.getResult[0],
            numerator: this.getResult[1],
            denominator: this.getResult[2],
          },
        });
        this.saveID++;
        this.updateLocalstorage();

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

        this.fractionsQuantity = 2;
      }
    },
    updateLocalstorage() {
      const serializedFractions = JSON.stringify(this.savedFractions);
      localStorage.setItem("fractions", serializedFractions);
    },
    deleteCalculation(index) {
      this.savedFractions.splice(index, 1);
      this.updateLocalstorage();
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
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@600&family=Roboto&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Open Sans", sans-serif;
}

.wrapper {
  margin: 0 auto;
  max-width: 950px;
  padding: 50px 15px;
}

.input-fractions {
  display: flex;
  &__list {
    display: flex;
  }
  &__item {
    position: relative;
    list-style: none;
  }
  &__equal {
    margin: 0 20px;
  }
  &__fraction {
    position: relative;
  }
  &__cross {
    position: absolute;
    top: -20px;
    right: 0;
  }
}

.result {
  display: flex;
  align-items: center;
  font-weight: 600;
}

.main {
  padding: 40px 30px;
  border: 1px solid #f2f2f2;
  box-shadow: 0px 1px 8px rgba(64, 42, 22, 0.03);
  border-radius: 8px;
}

.calculator {
  margin-bottom: 38px;
  &__input-fractions {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  &__title {
    margin-bottom: 22px;
  }
  &__new-fraction-btn {
    margin-bottom: 20px;
  }
}

.fractional {
  margin-left: 10px;
  &__item {
    width: 59px;
    height: 48px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}

.line {
  width: 59px;
  height: 1px;
  background: #383129;
  margin: 10px 0;
}

.calculations {
  &__title {
    margin-bottom: 10px;
  }
  &__default-text {
    text-align: center;
    font-weight: 400;
    font-size: 16px;
    color: #9d9c9c;
  }
  &__expression {
    display: flex;
    align-items: center;
  }
}

.list {
  list-style: none;
  &__item {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
}

.expression {
  display: flex;
  &__item {
    display: flex;
    align-items: center;
  }
  &__plus {
    margin: 0 20px;
  }
  &__equal {
    margin: 0 20px;
  }
}
</style>
