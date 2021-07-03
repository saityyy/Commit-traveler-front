<template>
  <div id="app">
    <div id="header"><Header :userInfo="userInfo" /></div>
    <div id="main">
      <div id="map">
        <Map
          :mapEvent="mapEvent"
          :sidebarEvent="sidebarEvent"
          @MapEvent="receiveMapEvennt"
          @SidebarEvent="receiveSidebarEvent"
          :userInfo="userInfo"
        />
      </div>
      <div id="sidebar">
        <Sidebar
          :mapEvent="mapEvent"
          :sidebarEvent="sidebarEvent"
          @SidebarEvent="receiveSidebarEvent"
          :userInfo="userInfo"
          :showReversi="showReversi"
          :receiveReversiEvent="receiveReversiEvent"
        />
      </div>
    </div>
    <Reversi v-if="showReversi"
        :receiveReversiEvent="receiveReversiEvent"
        :selectLanguage="selectLanguage"
    />
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import Sidebar from "./components/Sidebar.vue";
import Header from "./components/Header.vue";
import Reversi from "./components/Reversi.vue"
export default {
  name: "App",
  components: {
    Header,
    Map,
    Sidebar,
    Reversi,
  },
  data: function () {
    return {
      mapEvent: {},
      sidebarEvent: {
        moveToNext: { userMovingFlag: false, nextNode: {} },
      },
      userInfo: {},
      selectLanguage: "blank",
      showReversi: false,
    };
  },
  beforeCreate() {
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
  watch: {
    sidebarEvent: {
      handler: function () {
        console.log("sidebarEvent");
        this.axios
          .get("http://localhost:3000/api/get-user")
          .then((res) => {
            this.userInfo = res.data[0];
          })
          .catch((e) => {
            console.log(e);
          });
      },
      deep: true,
    },
  },
  methods: {
    receiveMapEvennt: function (event) {
      this.mapEvent = event;
    },
    receiveSidebarEvent: function (event) {
      this.sidebarEvent = event;
      console.log(event);
    },
    receiveReversiEvent: function(v = "blank"){
      this.selectLanguage = v;
      this.showReversi = !this.showReversi;

    }
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
  position:relative;
}
#sidebar {
  height: 100%;
  width: 35%;
  float: right;
  border: solid;
}
</style>
