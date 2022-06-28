<template>
<div class="cross">
  <div class="cross__btn" @click="$emit('delete-fraction')">
      <div class="cross__line cross__line__upper"></div>
      <div class="cross__line cross__line__lower"></div>
  </div>
</div>
</template>

<script>
  export default {
    
  }
</script>

<style lang="scss" scoped>
.cross {
  margin-bottom: 30px;
  &__btn {
    position: relative;
    left: 20px;
  }
  &__line {
    width: 21.5px;
    height: 2px;
    background: #9D9C9C;
    &__upper {
      transform: rotate(45deg);
      
    }
    &__lower {
      position: relative;
      transform: rotate(-45deg);
      bottom: 1px;
    }
  }
}

</style>