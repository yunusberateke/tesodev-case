<template>
    <div class="custom-button" @click="$emit('search')">{{placeHolder}}</div>
</template>
<script>
export default {
  props:['placeHolder']
}
</script>
<style scoped>
.custom-button {
  width: 138px;
  height: 48px;
  background-color: #204080;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-left: 16px;
  border-radius: 8px;

  font-size: 18px;
  font-family: "Courier New", Courier, monospace;
  font-weight: 700;
}

.custom-button:hover {
  cursor: pointer;
  background-color: #4F75C2;
}
</style>