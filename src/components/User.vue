<template>
  <g
    id="user"
    fill="#ffffff00"
    v-on:mouseover="mouseOverAction"
    v-on:mouseleave="mouseLeaveAction"
    :stroke="color"
    stroke-width="3"
  >
    <circle :cx="user_x" :cy="user_y" r="26"></circle>
    <circle :cx="user_x + 10" :cy="user_y - 6" r="6"></circle>
    <circle :cx="user_x - 10" :cy="user_y - 6" r="6"></circle>
    <line :x1="user_x" :y1="user_y + 14" :x2="user_x - 12" :y2="user_y + 9" />
    <line :x1="user_x" :y1="user_y + 14" :x2="user_x + 12" :y2="user_y + 9" />
  </g>
</template>

<script>
export default {
  data: function () {
    return {
      color: "black",
    };
  },
  props: {
    user_x: Number,
    user_y: Number,
  },
  methods: {
    mouseOverAction() {
      this.color = "red";
    },
    mouseLeaveAction() {
      this.color = "black";
    },
  },
};
</script>

<style>
#node {
  border: solid;
}
</style>