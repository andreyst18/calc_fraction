<template>
  <button 
    @click="$emit('add-fraction')"
    class="add-fraction-btn"
  >Добавить дробь</button>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped>
.add-fraction-btn {
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
