<template>  
    <div></div>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {},
  created() {
    this.$http
      .get(
        "https://api.trello.com/1/boards/Uky0qCHP?fields=name,url&key=20496bbf6978138b3b3e5462eded71ea&token=879ba30c74eb091eb42c222d871559378006b2caa26b338c442585c3edada979"
      )
      .then(function(data) {
        console.log(data);
      });
  }
};
</script>

<style>
</style>
