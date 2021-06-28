<template>
  <svg id="graph">
    <svg v-for="node in mapdata" :key="node.id">
      <svg v-for="line in node.nextNode" :key="line.id">
        <Edge :x="line.x" :y="line.y" :x2="node.x" :y2="node.y" />
      </svg>
      <Node :nodeObj="node" />
    </svg>
  </svg>
</template>

<script>
import Node from "./Node.vue";
import Edge from "./Edge.vue";
import mapdata from "../assets/mapdata.json";

export default {
  data: function () {
    return {
      color: "white",
      mapdata: mapdata,
    };
  },
  components: {
    Node,
    Edge,
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
#graph {
  width: 100%;
  height: 100%;
}
</style>