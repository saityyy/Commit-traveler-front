<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <svg viewBox="0 0 300 300" width="300" height="300">
      <line x1="200" y1="100" x2="200" y2="200" stroke="black"></line>
      <circle cx="100" cy="200" r="50" fill="white" stroke="black"></circle>
      <rect
        :x="centerx - scope"
        :y="centery - scope"
        :width="2 * scope"
        :height="2 * scope"
        fill="#77777777"
      />
    </svg>
    <svg
      @mousemove="touchmove"
      @mousedown="canMove = true"
      @mouseup="canMove = false"
      :viewBox="viewbox"
      width="300"
      height="300"
    >
      <line x1="199" y1="100" x2="200" y2="200" stroke="black"></line>
      <circle cx="100" cy="200" r="50" fill="white" stroke="black"></circle>
    </svg>
    <div>
      <input type="range" v-model="scope" />
      <p>{{ scope }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data: function () {
    return {
      canMove: false,
      scope: 150,
      centerx: 150,
      centery: 150,
      prevx: 0,
      prevy: 0,
    };
  },
  props: {
    msg: String,
  },
  computed: {
    viewbox: function () {
      const viewbox = [
        parseInt(this.centerx) - parseInt(this.scope),
        parseInt(this.centery) - parseInt(this.scope),
        2 * this.scope,
        2 * this.scope,
      ].join(" ");
      console.log(viewbox);
      return viewbox;
    },
  },
  methods: {
    touchmove(e) {
      if (this.canMove) {
        this.centerx = e.offsetX;
        this.centery = e.offsetY;
        //this.centerx += this.prevx - e.offsetX;
        //this.centery += this.prevy - e.offsetY;
        //console.log(this.centerx);
        //this.prevx = this.centerx;
        //this.prevy = this.centery;
      }
    },
  },
};
</script>

<style>
</style>