<template>
  <div id="mapinfo">
    <div id="user_location" v-if="currentShowFlag">
      <h1>現在位置</h1>
      <h2>{{ current_node_id }}番ノード</h2>
      <h2>{{ current_node_type }}</h2>
      <h2>次に行けるノード</h2>
      <h3 v-for="node in current_nextNode" :key="node.id">
        {{ node.id }}番ノード
      </h3>
    </div>
    <div id="next_location" v-if="selectedShowFlag">
      <h2>{{ selected_node_id }}番ノード</h2>
      <h2>{{ selected_node_type }}</h2>
      <h2>次に行けるノード</h2>
      <h3 v-for="node in selected_nextNode" :key="node.id">
        {{ node.id }}番ノード
      </h3>
      <p>x: {{ selected_node_x }}</p>
      <p>y: {{ selected_node_y }}</p>
    </div>
  </div>
</template>

<script>
import mapdata from "../assets/mapdata.json";

export default {
  data: function () {
    return {
      selected_node_id: "",
      selected_node_type: "",
      selected_nextNode: [],
      selected_node_x: "",
      selected_node_y: "",
      selectedShowFlag: false,
      current_node_id: "",
      current_node_type: "",
      current_nextNode: [],
      current_node_x: "",
      current_node_y: "",
      currentShowFlag: false,
    };
  },
  props: {
    mapEvent: Object,
    userInfo: Object,
  },
  watch: {
    mapEvent(updatedInfo) {
      if (updatedInfo) {
        this.selected_node_id = updatedInfo.id;
        this.selected_node_type = updatedInfo.type;
        this.selected_node_x = updatedInfo.x;
        this.selected_node_y = updatedInfo.y;
        this.selected_nextNode = [];
        updatedInfo.nextNode.forEach((value) => {
          this.selected_nextNode.push(mapdata[value - 1]);
        });
        this.selectedShowFlag = true;
        this.currentShowFlag = false;
      }
    },
    userInfo(updatedInfo) {
      console.log(updatedInfo);
      if (updatedInfo) {
        this.current_node_id = updatedInfo.node_id;
        const _id = parseInt(this.current_node_id);
        this.current_node_type = mapdata[_id - 1].type;
        this.current_node_x = mapdata[_id - 1].x;
        this.current_node_y = mapdata[_id - 1].y;
        this.current_nextNode = [];
        mapdata[_id - 1].nextNode.forEach((value) => {
          this.current_nextNode.push(mapdata[value - 1]);
        });
        this.currentShowFlag = true;
      }
    },
  },
};
</script>

<style>
</style>