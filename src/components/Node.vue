<template>
  <g
    :id="nodeObj.id"
    @dblclick="mouseDblclickAction"
    :fill="color"
    stroke="black"
    stroke-width="4"
  >
    <circle :cx="nodeObj.x" :cy="nodeObj.y" r="30"></circle>
    <circle
      v-if="nodeObj.type == 'checkpoint'"
      :cx="nodeObj.x"
      :cy="nodeObj.y"
      r="20"
    ></circle>
  </g>
</template>

<script>
export default {
  data: function () {
    return {
      color: "white",
      eventFlag: false,
    };
  },
  props: {
    nodeObj: Object,
    nodeEventId: String,
  },
  watch: {
    nodeEventId(updatedInfo) {
      if (updatedInfo == this.nodeObj.id) {
        this.eventFlag = true;
        this.color = "red";
      } else {
        this.eventFlag = false;
        this.color = "white";
      }
    },
  },
  methods: {
    //mouseOverAction() {
    //this.$emit("selectedNode", this.nodeObj);
    //},
    //mouseLeaveAction() {
    //this.color = "white";
    //this.$emit("selectedNode", null);
    //},
    mouseDblclickAction() {
      this.$emit("selectedNode", this.nodeObj);
    },
  },
};
</script>

<style>
#node {
  border: solid;
}
</style>