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
      最前面にノードを置くので最後に記述する
      :keyはnode.idがすでに使用済みなので少し変えておく
    -->
    <g v-for="node in mapdata" :key="'_' + node.id">
      <Node
        :nodeObj="node"
        :nodeEventId="nodeEventId"
        @selectedNode="selectedNode"
      />
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
    <User :user_x="user_x" :user_y="user_y" :moveFlag="userMoveFlag" />
  </svg>
</template>

<script>
import Node from "./Node.vue";
import Edge from "./Edge.vue";
import User from "./User.vue";
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
      nodeEventId: "",
      userMoveFlag: false,
      user_x: -10000,
      user_y: -10000,
    };
  },
  components: {
    Node,
    Edge,
    User,
  },
  props: {
    mapEvent: Object,
    sidebarEvent: Object,
    userInfo: Object,
  },
  created() {
    //マップデータベース更新
    this.axios
      .post("http://localhost:3000/api/update-map", mapdata)
      .then((res) => {
        console.log(res.data);
      })
      .catch((e) => {
        console.log(e);
      });
  },
  //変数が変更された時にメソッドを呼び出す
  watch: {
    userInfo(updatedInfo) {
      console.log("Map.vue 112");
      console.log(updatedInfo);
      this.user_x = parseInt(mapdata[parseInt(updatedInfo.node_id) - 1].x);
      this.user_y = parseInt(mapdata[parseInt(updatedInfo.node_id) - 1].y);
    },
    sidebarEvent: {
      handler: function (updatedInfo) {
        if (this.sidebarEvent.moveToNext.userMovingFlag) {
          this.userMoveFlag = true;
          this.moveUser(updatedInfo.moveToNext.nextNode);
        }
      },
      deep: true,
    },
  },
  //watchとほぼ同じだが、こっちは監視する変数を指定する必要がない
  computed: {
    viewbox: function () {
      const viewScale = parseInt(this.viewScale);
      const minx = parseInt(this.centerx) - viewScale;
      const miny = parseInt(this.centery) - viewScale;
      const viewbox = [minx, miny, 2 * viewScale, 2 * viewScale].join(" ");
      return viewbox;
    },
    step: function () {
      return this.userInfo.step;
    },
    commit_count: function () {
      return this.userInfo.commit;
    },
  },
  methods: {
    //実装汚い
    moveUser(nextNode) {
      console.log("moveUser");
      const dx = nextNode.x - this.user_x;
      const dy = nextNode.y - this.user_y;
      const current_x = this.user_x;
      const current_y = this.user_y;
      var sum_x = 0;
      var sum_y = 0;
      var count = 0;
      const moveMilliSecond = 4000;
      var move = setInterval(
        function () {
          count += 1;
          console.log("setInterval");
          this.user_x = 300;
          sum_x += dx / 100.0;
          sum_y += dy / 100.0;
          this.user_x = current_x + Math.floor(sum_x);
          this.user_y = current_y + Math.floor(sum_y);
          if (count >= 100) {
            console.log("setTimeout");
            this.user_x = parseInt(nextNode.x);
            this.user_y = parseInt(nextNode.y);
            this.endMove();
            clearInterval(move);
          }
        }.bind(this), //vueのデータを参照するには.bind(this)をつける
        moveMilliSecond / 100
      );
    },
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
      this.$emit("MapEvent", selectedNodeInfo);
      this.nodeEvent = selectedNodeInfo;
      this.nodeEventId = selectedNodeInfo.id;
    },
    endMove() {
      this.userMoveFlag = false;
      this.sidebarEvent.moveToNext.userMovingFlag = false;
      this.$emit("SidebarEvent", this.sidebarEvent);
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