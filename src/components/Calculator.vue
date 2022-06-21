<template>
  <section class="section calculator">
    <div class="calculator__input-fractions input-fractions">
      <Fraction></Fraction>
      <Fraction></Fraction>
    </div>
    <AddFractionBtn></AddFractionBtn>
    <SaveCalcBtn></SaveCalcBtn>
  </section>
</template>

<script>
import Fraction from './Fraction.vue'

export default {
  components: {
    Fraction
  },
  data() {
    return {
      fractions: [],
    };
  },
};
</script>

<style lang="scss" scoped>
  .input-fractions {
    display: flex;
  }
</style>
