<template>
  <div>
    <input :class="{error:error}"  type="text" v-model="searchQueryModel" placeholder="Search" />
    <label v-if="error">{{errorMessage}}</label>
  </div>
</template>

<script>
export default {
  props:['error', 'errorMessage', 'searchQuery'],
  
  data(){
    return{
      searchQueryModel:this.searchQuery,
    }
  },

  watch: {
    searchQueryModel(){
      this.$emit('update:searchQuery', this.searchQueryModel);
      if (this.error) 
        this.$emit('update:error', false);
      
    }
  }
}
</script>

<style scoped>
div {
  display: flex;
  flex-direction: column;
}

label {
  padding-top: 5px;
  padding-left: 5px;
  font-weight: bold;
  font-size: 12px;
  line-height: 21px;
  color: rgba(255, 0, 0, 0.5);
}
input {
  width: 709px;
  border-radius: 8px;
  border: 2px solid #000000;
  font-size: 18px;
  line-height: 21px;
  font-weight: 700;
  font-family: "Courier New", Courier, monospace;
  color: #888888;
  padding: 10px;
}

input:hover {
  background: rgba(100, 100, 100, 0.08);
  border: 2px solid rgba(0, 0, 0, 0.6);
}

.error {
  border: 2px solid #ff0000 !important;

  font-weight: bold;
  font-size: 18px;
  color: rgba(255, 0, 0, 0.5);
}
</style>