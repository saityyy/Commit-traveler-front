<template>
  <g>
    <marker id="arrow" viewBox="0 -300 400 700" orient="auto">
      <polygon points="200,-300 700,0 200,300" fill="black" stroke="none" />
    </marker>
    <line
      :x1="fromx"
      :y1="fromy"
      :x2="tox"
      :y2="toy"
      stroke="black"
      stroke-width="4"
    ></line>
    <line
      :x1="(parseInt(fromx) + parseInt(tox)) / 2"
      :y1="(parseInt(fromy) + parseInt(toy)) / 2"
      :x2="tox"
      :y2="toy"
      stroke="black"
      stroke-width="4"
      marker-start="url(#arrow)"
    ></line>
  </g>
</template>

<script>
export default {
  props: {
    fromx: String,
    fromy: String,
    tox: String,
    toy: String,
  },
};
</script>

<style>
</style>