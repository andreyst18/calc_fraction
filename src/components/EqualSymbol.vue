<template>
  <div class="equal">
    <div class="equal__line"></div>
    <div class="equal__line"></div>
  </div>
</template>

<script>
  export default {
    
  }
</script>

<style lang="scss" scoped>
.equal {
  &__line {
    width: 17px;
    height: 1px;
    background: #383129;
    &:last-child {
      margin-top: 6px;
    }
  }
}
</style>