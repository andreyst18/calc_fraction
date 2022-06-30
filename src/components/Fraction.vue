<template>
  <div class="fraction">
    <slot></slot>
    <PlusSymbol class="fraction__plus" v-if="!isFirst"></PlusSymbol>
    <div class="fraction__inputs">
      <input
      class="fraction__item"
      type="text"
      v-model="numerator"
      @change="checkInputValue(numerator, true)"
      placeholder="00"
    />
    <div class="fraction__line"></div>
    <input
      class="fraction__item"
      type="text"
      v-model="denominator"
      @change="checkInputValue(denominator, false)"
      placeholder="00"
    />
    </div>
    
  </div>
</template>

<script>
import PlusSymbol from "./PlusSymbol.vue";

export default {
  props: {
    isFirst: Boolean,
    index: Number,
    id: Number,
  },
  components: {
    PlusSymbol,
  },
  data() {
    return {
      numerator: "",
      denominator: "",
    };
  },
  methods: {
    checkInputValue(value, isNumerator) {
      value = +value;
      if (!(value > 0 && value < 100 && value === Math.floor(value))) {
        alert("Неверное значение. Введите целое число от 1 до 99.");
        if (isNumerator) {
          this.numerator = "";
        } else {
          this.denominator = "";
        }
      } else {
        this.getCompleteFraction();
      }
    },
    getCompleteFraction() {
      const args = [this.numerator, this.denominator, this.index, this.id];
      if (this.numerator && this.denominator) {
        this.$emit("add-fraction", ...args);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font: {
      family: 'Open Sans';
      size: 16px; 
      weight: 400
    }
}

.fraction {
  display: flex;
  align-items: center;
  &__line {
    width: 59px;
    height: 1px;
    background: #383129;
    margin: 10px 0px;
  }
  &__item {
    width: 59px;
    height: 48px;
    text-align: center;
    border: 1px solid #CBC9C9;
    border-radius: 5px;
    &:hover {
      border: 1px solid #9D9C9C;
    }
    &:active {
      border: 1px solid #383129;
    }
  }
  &__plus {
    margin: 0 20px;
  }
}
</style>
