<template>
  <div id="app">
    <div id="header"><Header /></div>
    <div id="main">
      <div id="map">
        <Map />
      </div>
      <div id="sidebar">
        <Sidebar />
      </div>
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import Sidebar from "./components/Sidebar.vue";
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    Header,
    Map,
    Sidebar,
  },
};
</script>

<style>
html,
body {
  margin: 0px;
}
#app {
  position: absolute;
  width: 100%;
  height: 100%;
}
#main {
  position: absolute;
  width: 70%;
  height: 80%;
  margin-left: 15%;
  margin-right: 15%;
}
#map {
  height: 100%;
  width: 70%;
  float: left;
  border: solid;
}
#sidebar {
  height: 100%;
  width: 26%;
  margin-right: 1%;
  margin-left: 1%;
  float: right;
  border: solid;
}
</style>
