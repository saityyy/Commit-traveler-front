<template>
  <div id="mapinfo">
    <p>{{ mapEvent }}</p>
    <p>{{ node_id }}</p>
    <p>{{ node_type }}</p>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      node_id: "",
      node_type: "",
    };
  },
  props: {
    mapEvent: Object,
  },
  watch: {
    mapEvent(updatedInfo) {
      console.log(updatedInfo);
      if (updatedInfo) {
        this.node_id = updatedInfo.id;
        this.node_type = updatedInfo.type;
      }
    },
  },
};
</script>

<style>
</style>