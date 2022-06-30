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

.delete-btn {
  font-family: "Open Sans", sans-serif;
  padding: 13px 20px;
  background: #fff;
  border-radius: 5px;
  border: 1px dashed #CBC9C9;
  color: #383129;
  cursor: pointer;
  &:hover {
    background: #F2F2F2;
  }
  &:active {
    background: #E0DEDE;
  }
}

</style>