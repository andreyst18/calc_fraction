<template>
  <button 
    @click="$emit('save-calc')"
    class="save-calc-btn"
  >Сохранить расчёты</button>
</template>

<script>
  export default {
    
  }
</script>

<style lang="scss" scoped>
.save-calc-btn {
  font-family: "Open Sans", sans-serif;
  padding: 13px 20px;
  background: #FF962B;
  border-radius: 5px;
  border: none;
  color: #fff;
  cursor: pointer;
  &:hover {
    background: #F88D20;
  }
  &:active {
    background: #ED7D0B;
  }
}
</style>