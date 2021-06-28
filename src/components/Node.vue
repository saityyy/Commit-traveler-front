<template>
  <svg :id="nodeObj.id">
    <circle
      v-on:mouseover="mouseOverAction"
      v-on:mouseleave="mouseLeaveAction"
      :cx="nodeObj.x"
      :cy="nodeObj.y"
      r="30"
      :fill="color"
      stroke="black"
    ></circle>
  </svg>
</template>

<script>
export default {
  data: function () {
    return {
      color: "white",
    };
  },
  props: {
    nodeObj: Object,
  },
  methods: {
    mouseOverAction() {
      this.color = "black";
    },
    mouseLeaveAction() {
      this.color = "white";
    },
  },
};
</script>

<style>
#node {
  border: solid;
}
</style>