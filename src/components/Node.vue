<template>
  <g :id="nodeObj.id">
    <circle
      v-on:mouseover="mouseOverAction"
      v-on:mouseleave="mouseLeaveAction"
      :cx="nodeObj.x"
      :cy="nodeObj.y"
      r="30"
      :fill="color"
      stroke="black"
      stroke-width="4"
    ></circle>
    <circle
      v-if="nodeObj.type == 'checkpoint'"
      v-on:mouseover="mouseOverAction"
      v-on:mouseleave="mouseLeaveAction"
      :cx="nodeObj.x"
      :cy="nodeObj.y"
      r="20"
      :fill="color"
      stroke="black"
      stroke-width="4"
    ></circle>
  </g>
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
      this.color = "#999999";
      this.$emit("selectedNode", this.nodeObj);
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