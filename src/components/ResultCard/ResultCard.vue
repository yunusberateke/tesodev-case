<template>
  <div class="main">
    <div class="result">
      <div class="title-and-date">
        <p class="title">{{title}}</p>
        <p class="date">{{date}}</p>
      </div>
      <div class="email">Email: {{email}}</div>
    </div>
    <hr />
  </div>
</template>

<script>
export default {
    props:['title', 'date', 'email']
};
</script>

<style scoped>

.result {
  display: flex;
  justify-content: space-between;
  padding: 9px 20px;
}

.result .title {
  font-size: 18px;
  font-weight: 700;
  line-height: 21px;
}

.result .date {
  font-size: 12px;
  font-weight: 700;
  line-height: 14px;
}

.result .email {
  font-size: 18px;
  font-weight: 700;
  line-height: 21px;
}

.result:hover {
    background-color: #C4C4C4;
}
</style>