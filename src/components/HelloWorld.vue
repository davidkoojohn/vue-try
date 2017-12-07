<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      msg: 'Welcome koo',
    };
  },
};
</script>

<style scoped>
.hello h1 {
  text-align: center;
}
</style>
