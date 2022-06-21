<template>
  <div class="fraction">
    <input class="fraction__item" type="text" v-model="numerator">
    <div class="fraction__line"></div>
    <input class="fraction__item" type="text" v-model="denominator">
  </div>
</template>

<script>
  export default {
    data() {
      return {
        numerator: '',
        denominator: ''
      }
    }
  }
</script>

<style lang="scss" scoped>
.fraction {
  &__line {
    width: 59px;
    height: 1px;
    background: #383129;
;
  }
}

</style>