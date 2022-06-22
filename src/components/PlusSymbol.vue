<template>
  <div class="plus">
    <div class="plus__line"></div>
    <div class="plus__line plus__line-vertical"></div>
  </div>
</template>

<script>
  export default {
    
  }
</script>

<style lang="scss" scoped>
.plus {
  &__line {
    width: 17px;
    height: 1px;
    background: #383129;
    &-vertical {
      transform: rotate(90deg);
    }
  }
}
</style>