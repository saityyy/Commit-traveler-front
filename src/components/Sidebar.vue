<template>
  <div class="tab_container">
    <input id="tab1" type="radio" name="tab_item" checked/>
    <label class="tab_item" for="tab1">じょうほう</label>
    <!--
    <input id="tab2" type="radio" name="tab_item" checked />
    <label class="tab_item" for="tab2">マップ</label>
    -->
    <input id="tab3" type="radio" name="tab_item" />
    <label class="tab_item" for="tab3">こうどう</label>
    <input id="tab4" type="radio" name="tab_item" />
    <label class="tab_item" for="tab4">ランキング</label>
    <div class="tab_content" id="tab1_content">
      <div class="tab_content_description">
        <UserInfo :userInfo="userInfo" />
        <div style="text-align:center;" @click="viewReversiEvent"><button id="view_reversi_btn">リバーシを見る</button></div>
      </div>
    </div>
    <!--
    <div class="tab_content" id="tab2_content">
      <div class="tab_content_description">
        <MapInfo :mapEvent="mapEvent" :userInfo="userInfo" />
      </div>
    </div>
    -->
    <div class="tab_content" id="tab3_content">
      <div class="tab_content_description">
        <CommitInfo
          @moveToNext="moveToNext"
          :sidebarEvent="sidebarEvent"
          :userInfo="userInfo"
          :receiveReversiEvent="receiveReversiEvent"
        />
      </div>
    </div>
    <div class="tab_content" id="tab4_content">
      <div class="tab_content_description">
        <RankingInfo />
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import MapInfo from "./MapInfo.vue";
import UserInfo from "./UserInfo.vue";
import RankingInfo from "./RankingInfo.vue";
import CommitInfo from "./CommitInfo.vue";

export default {
  props: {
    mapEvent: Object,
    sidebarEvent: Object,
    userInfo: Object,
    showReversi: Boolean,
    receiveReversiEvent: Function,
    viewReversiEvent: Function,
  },
  components: {
    MapInfo,
    UserInfo,
    RankingInfo,
    CommitInfo,
  },
  data: function () {
    return {};
  },
  methods: {
    moveToNext(event) {
      this.$emit("SidebarEvent", event);
    },
  },
};
</script>

<style>
button:hover{
opacity: 0.6;
transition: 0.2s;
}
button{
  background-color:#37beb0;
  padding:10px;
  border:solid 3px #c3c3c3;
}
#view_reversi_btn{
  margin-top:100px;
}
.tab_container {
  padding-bottom: 1em;
  background-color: #fff;
  margin: 0 auto;
}
.tab_item {
  font-size: 12px;
  width: calc(100% / 3);
  padding: 15px 0;
  border-bottom: 3px solid #37beb0;
  background-color: #ececec;
  text-align: center;
  color: #37beb0;
  display: block;
  float: left;
  text-align: center;
  font-weight: bold;
  transition: all 0.2s ease;
}
.tab_item:hover {
  opacity: 0.75;
}
input[name="tab_item"] {
  display: none;
}
.tab_content {
  display: none;
  padding: 1em 1em 0;
  clear: both;
  overflow: hidden;
}
#tab1:checked ~ #tab1_content,
#tab2:checked ~ #tab2_content,
#tab3:checked ~ #tab3_content,
#tab4:checked ~ #tab4_content {
  display: block;
}
.tab_container input:checked + .tab_item {
  background-color: #37beb0;
  color: #fff;
}
</style>