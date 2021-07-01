<template>
  <div id="app">
    <div id="header"><Header /></div>
    <input type="button" value="コミットを反映させる" />
    <div id="main">
      <div id="map">
        <Map @commitInfo="commitInfo" :userInfo="userInfo" />
      </div>
      <div id="sidebar">
        <Sidebar :mapEvent="mapEvent" />
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
  data: function () {
    return {
      mapEvent: {},
      userInfo: {},
    };
  },
  methods: {
    commitInfo: function (info) {
      this.mapEvent = info;
    },
  },
  beforeCreate() {
    //userのコミット情報を取得する
    this.axios
      .get("http://localhost:3000/api/get-commit")
      .then((res) => {
        console.log(res.data);
      })
      .catch((e) => {
        console.log(e);
      });
    //userの情報を取得する
    this.axios
      .get("http://localhost:3000/api/get-user")
      .then((res) => {
        this.userInfo = res.data[0];
      })
      .catch((e) => {
        console.log(e);
      });
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
  min-width: 800px;
}
#main {
  position: absolute;
  width: 90%;
  height: 700px;
  margin-left: 5%;
  margin-right: 5%;
}
#map {
  height: 100%;
  width: 60%;
  float: left;
  border: solid;
}
#sidebar {
  height: 100%;
  width: 35%;
  margin-right: 1%;
  margin-left: 1%;
  float: right;
  border: solid;
}
</style>
