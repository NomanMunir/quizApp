<template>
  <div>
    <nav>
      <a href="#" class="disabled">Fancy Quiz</a>
      <a href="#" class="disabled">Counter: {{ this.numCurrect }}/{{ numTotal }}</a>
    </nav>
  </div>
</template>

<script>
export default {
  props: {
    numCurrect: Number,
    numTotal: Number,
  },
};
</script>

<style scoped>
nav {
  font-family: Arial, Helvetica, sans-serif;
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 10px;
  border-bottom: 1px solid #979797;
}
nav a {
  color: #979797;
  text-decoration: none;
  padding: 10px 20px;
  text-align: center;
}
.disabled {
  color: #817373;
}
</style>