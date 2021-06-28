<template>
  <svg id="graph">
    <svg v-for="node in mapdata" :key="node.id">
      <svg v-for="next_node in node.nextNode" :key="next_node">
        <Edge
          :x="mapdata[next_node - 1].x"
          :y="mapdata[next_node - 1].y"
          :x2="node.x"
          :y2="node.y"
        />
      </svg>
    </svg>
    <!-- 
      最前面にノードを置くので最後に配置する
      :keyはnode.idがすでに使用済みなので少し変えておく
    -->
    <svg v-for="node in mapdata" :key="'_' + node.id">
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