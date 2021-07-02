<template>
  <div id="mapinfo" v-if="showFlag">
    <p>選択中のノード</p>
    <p>{{ node_id }}</p>
    <p>タイプ</p>
    <p>{{ node_type }}</p>
    <p>場所</p>
    <p>x: {{ node_x }}</p>
    <p>y: {{ node_y }}</p>
    <br />
    <p>次のノード</p>
    <p v-for="node in nextNode" :key="node.id">{{ node.id }}</p>
  </div>
</template>

<script>
import mapdata from "../assets/mapdata.json";

export default {
  data: function () {
    return {
      node_id: "",
      node_type: "",
      nextNode: [],
      showFlag: false,
    };
  },
  props: {
    mapEvent: Object,
  },
  watch: {
    mapEvent(updatedInfo) {
      if (updatedInfo) {
        this.node_id = updatedInfo.id;
        this.node_type = updatedInfo.type;
        this.nextNode = [];
        updatedInfo.nextNode.forEach((value) => {
          this.nextNode.push(mapdata[value - 1]);
        });
        this.showFlag = true;
      }
    },
  },
};
</script>

<style>
</style>