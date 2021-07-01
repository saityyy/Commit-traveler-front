<template>
  <svg
    id="graph"
    :viewBox="viewbox"
    preserveAspectRatio="xMidYMid meet"
    @mouseup="canMove = false"
    @mousedown="canMove = true"
    @mousemove="moveMap"
    @mouseleave="canMove = false"
    @wheel="mouseWheelAction"
  >
    <g v-for="node in mapdata" :key="node.id">
      <g v-for="next_node in node.nextNode" :key="next_node">
        <Edge
          :fromNode="node"
          :toNode="mapdata[next_node - 1]"
          :nodeEvent="nodeEvent"
        />
      </g>
    </g>
    <!-- 
      最前面にノードを置くので最後に配置する
      :keyはnode.idがすでに使用済みなので少し変えておく
    -->
    <g v-for="node in mapdata" :key="'_' + node.id">
      <Node :nodeObj="node" @selectedNode="selectedNode" />
    </g>
    <!--中心点-->
    <circle :cx="centerx" :cy="centery" r="5" />
    <!--マップの外枠-->
    <line x1="0" y1="0" x2="100000" y2="0" stroke="black" />
    <line x1="0" y1="0" x2="0" y2="100000" stroke="black" />
    <line
      x1="0"
      :y1="bottomMap"
      :x2="rightMap"
      :y2="bottomMap"
      stroke="black"
    />
    <line :x1="rightMap" y1="0" :x2="rightMap" :y2="bottomMap" stroke="black" />
    <rect
      x="0"
      :y="bottomMap"
      width="100000"
      height="100000"
      fill="#999999bb"
    />
    <rect
      :x="rightMap"
      y="0"
      width="100000"
      :height="bottomMap"
      fill="#999999bb"
    />
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
      bottomMap: 1000,
      rightMap: 1000,
      canMove: false,
      nodeEvent: null,
    };
  },
  components: {
    Node,
    Edge,
  },
  created() {
    this.axios
      .post("http://localhost:3000/api/update-map", mapdata)
      .then((res) => {
        console.log(res.data);
      })
      .catch((e) => {
        alert(e);
      });
  },
  computed: {
    viewbox: function () {
      const viewScale = parseInt(this.viewScale);
      const minx = parseInt(this.centerx) - viewScale;
      const miny = parseInt(this.centery) - viewScale;
      const viewbox = [minx, miny, 2 * viewScale, 2 * viewScale].join(" ");
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
      this.viewScale = parseInt(this.viewScale);
      if (e.deltaY > 0) this.viewScale += 10;
      else this.viewScale -= 10;
      if (this.viewScale < 150) this.viewScale = 150;
      const limitZoom = Math.min(this.rightMap, this.bottomMap);
      if (this.viewScale > limitZoom) this.viewScale = limitZoom;
    },
    moveMap(e) {
      if (this.canMove) {
        const newx = this.centerx - (this.viewScale * e.movementX) / 300;
        const newy = this.centery - (this.viewScale * e.movementY) / 300;
        this.centerx = Math.min(Math.max(0, newx), this.rightMap);
        this.centery = Math.min(Math.max(0, newy), this.bottomMap);
      }
    },
    selectedNode(selectedNodeInfo) {
      this.$emit("commitInfo", selectedNodeInfo);
      this.nodeEvent = selectedNodeInfo;
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