<template>
  <div>
    <button class="delete-btn" @click="$emit('delete-calculation', index)">Удалить</button>
  </div>
</template>

<script>
  export default {
    props: {
      index: Number
    }
    
  }
</script>

<style lang="scss" scoped>

</style>