<template>
  <g
    :id="node_id"
    @dblclick="mouseDblclickAction"
    :fill="color"
    stroke="black"
    stroke-width="4"
  >
    <circle :cx="node_x" :cy="node_y" r="30"></circle>
    <circle
      v-if="node_type == 'checkpoint'"
      :cx="node_x"
      :cy="node_y"
      r="20"
    ></circle>
    <text :x="node_x - 40" :y="node_y - 40" font-size="30">{{ node_id }}</text>
    <text
      v-if="node_type == 'start'"
      :x="node_x - 23"
      :y="node_y + 7"
      font-size="20"
      stroke-width="2"
      >start</text
    >
  </g>
</template>

<script>
export default {
  data: function () {
    return {
      color: "white",
      node_id: "",
      node_type: "",
      node_x: -10000,
      node_y: -10000,
    };
  },
  props: {
    nodeObj: Object,
    nodeEventId: String,
  },
  watch: {
    nodeEventId(updatedInfo) {
      if (updatedInfo == this.nodeObj.id) {
        this.color = "red";
      } else {
        this.color = "white";
      }
    },
  },
  created() {
    this.node_id = this.nodeObj.id;
    this.node_type = this.nodeObj.type;
    this.node_x = parseInt(this.nodeObj.x);
    this.node_y = parseInt(this.nodeObj.y);
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