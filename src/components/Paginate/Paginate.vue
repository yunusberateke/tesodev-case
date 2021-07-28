<template>
  <div class="paginate">
    <div class="left-right-button" @click="previousPaginate">Previous</div>
    <div class="paginate-items">
      <paginate-item
        :value="key + 1"
        :select="currentPaginate == key + 1"
        v-for="(val, key) in new Array(this.paginateCount)"
        :key="key"
        @changePaginate="changePaginate(key + 1)"
      />
    </div>
    <div class="left-right-button" @click="nextPaginate">Next</div>
  </div>
</template>

<script>
import PaginateItem from "../PaginateItem/PaginateItem.vue";
export default {
  components: { PaginateItem },
  props: ["paginateCount", "currentPaginate"],
  methods: {
    previousPaginate() {
      if(this.currentPaginate <= 1)
        return

      this.$emit("update:currentPaginate", this.currentPaginate - 1);
    },
    nextPaginate() {
      if(this.currentPaginate >= this.paginateCount)
        return

      this.$emit("update:currentPaginate", this.currentPaginate + 1);
    },
    changePaginate(paginate) {
      this.$emit("update:currentPaginate", paginate);
    },
  },
};
</script>

<style scoped>
.paginate {
  padding: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.left-right-button {
  width: 85.71px;
  height: 25px;
  border: 1px solid #484848;
  border-radius: 4px;

  font-weight: bold;
  font-size: 14px;
  line-height: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 10px;
}

.left-right-button:hover {
  cursor: pointer;
  background-color: #20408046;
}

.paginate-items {
  display: flex;
}
</style>