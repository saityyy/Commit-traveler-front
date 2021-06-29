<template>
  <g>
    <marker
      :id="fromNode.id + toNode.id"
      viewBox="0 -200 600 400"
      orient="auto"
    >
      <polygon points="200,-300 700,0 200,300" :fill="Color" stroke="none" />
    </marker>
    <line
      :x1="fromNode.x"
      :y1="fromNode.y"
      :x2="toNode.x"
      :y2="toNode.y"
      :stroke="Color"
      stroke-width="4"
    ></line>
    <line
      :x1="(parseInt(fromNode.x) + parseInt(toNode.x)) / 2"
      :y1="(parseInt(fromNode.y) + parseInt(toNode.y)) / 2"
      :x2="toNode.x"
      :y2="toNode.y"
      :stroke="Color"
      stroke-width="4"
      :marker-start="'url(#' + fromNode.id + toNode.id + ')'"
    ></line>
  </g>
</template>

<script>
export default {
  props: {
    fromNode: Object,
    toNode: Object,
    nodeEvent: Object,
  },
  computed: {
    Color: function () {
      if (this.nodeEvent === null) return "black";
      //双方向連結かどうかの判定
      else if (
        this.toNode.id == this.nodeEvent.id &&
        this.nodeEvent.nextNode.includes(parseInt(this.fromNode.id.slice(-1)))
      ) {
        return "red";
      }
      //単方向連結かどうかの判定
      else if (this.nodeEvent.id == this.fromNode.id) {
        return "red";
      } else {
        return "black";
      }
    },
  },
};
</script>

<style>
</style>