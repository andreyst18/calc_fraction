<template>
  <div class="fraction">
    <slot></slot>
    <PlusSymbol class="input-fractions__plus" v-if="!isFirst"></PlusSymbol>
    <input
      class="fraction__item"
      type="text"
      v-model="numerator"
      @change="checkInputValue(numerator, true)"
    />
    <div class="fraction__line"></div>
    <input
      class="fraction__item"
      type="text"
      v-model="denominator"
      @change="checkInputValue(denominator, false)"
    />
  </div>
</template>

<script>

import PlusSymbol from "./PlusSymbol.vue";

export default {
  props: {
    isFirst: Boolean,
    index: Number,
    id: Number
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
        console.log(this.numerator)
        this.$emit("add-fraction", ...args);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.fraction {
  &__line {
    width: 59px;
    height: 1px;
    background: #383129;
  }
  &__item {
    width: 40px;
  }
}
</style>
