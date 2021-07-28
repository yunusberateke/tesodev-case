<template>
  <div
    :class="{
      paginateItem: true,
      select: select,
    }"
    @click="$emit('changePaginate')"
  >
    {{ value }}
  </div>
</template>

<script>
export default {
  props: ["value", "select"],
};
</script>

<style scoped>
.paginateItem {
  width: 24px;
  height: 24px;
  border: 1px solid #484848;
  border-radius: 4px;
  display: flex;
  justify-content: center;
  align-items: center;

  font-weight: bold;
  font-size: 14px;
  line-height: 16px;
  margin-left: 2px;
  margin-right: 2px;
}

.select {
  background-color: #204080;
  color: #ffffff;
}

.paginateItem:hover {
  cursor: pointer;
  background-color: #20408046;
}
</style>