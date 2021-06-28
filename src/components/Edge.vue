<template>
  <svg>
    <line :x1="x" :y1="y" :x2="x2" :y2="y2" stroke="black"></line>
  </svg>
</template>

<script>
export default {
  props: {
    x: String,
    y: String,
    x2: String,
    y2: String,
  },
};
</script>

<style>
</style>