<template>
  <svg
    id="graph"
    :viewBox="viewbox"
    preserveAspectRatio="xMidYMid meet"
    @mouseup="canMove = false"
    @mousedown="canMove = true"
    @mousemove="moveMap"
    @wheel="mouseWheelAction"
  >
    <g v-for="node in mapdata" :key="node.id">
      <g v-for="next_node in node.nextNode" :key="next_node">
        <Edge
          :x="mapdata[next_node - 1].x"
          :y="mapdata[next_node - 1].y"
          :x2="node.x"
          :y2="node.y"
        />
      </g>
    </g>
    <!-- 
      最前面にノードを置くので最後に配置する
      :keyはnode.idがすでに使用済みなので少し変えておく
    -->
    <g v-for="node in mapdata" :key="'_' + node.id">
      <Node :nodeObj="node" />
    </g>
    <!--中心点-->
    <circle :cx="centerx" :cy="centery" r="5" />
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
      viewScale: 500,
      mapdata: mapdata,
      centerx: 400,
      centery: 400,
      prevx: this.centerx,
      prevy: this.centery,
      canMove: false,
    };
  },
  components: {
    Node,
    Edge,
  },
  computed: {
    viewbox: function () {
      const viewScale = parseInt(this.viewScale);
      const viewbox = [
        parseInt(this.centerx) - viewScale,
        parseInt(this.centery) - viewScale,
        2 * viewScale,
        2 * viewScale,
      ].join(" ");
      console.log(viewbox);
      return viewbox;
    },
  },
  methods: {
    mouseOverAction() {
      this.color = "black";
    },
    mouseLeaveAction() {
      this.color = "white";
    },
    mouseWheelAction(e) {
      console.log(document.getElementById("graph"));
      this.viewScale = parseInt(this.viewScale);
      if (e.deltaY > 0) this.viewScale += 10;
      else this.viewScale -= 10;
    },
    moveMap(e) {
      if (this.canMove) {
        this.centerx -= e.movementX;
        this.centery -= e.movementY;
      }
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