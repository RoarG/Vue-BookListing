<template>
    <li>{{items.url}}:{{items.id}}</li>
</template>

<script>
export default {
  name: "BookItem",
  props: ["book"]
};
</script>

<style>
li {
  display: block;
  margin: 0 10px;
}
</style>
