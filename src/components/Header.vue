<template>
  <div class="header">
    <div class="mark">codeGo</div>
    <img
      class="logo"
      :src="'https://avatars.githubusercontent.com/' + userInfo.name + '?s=500'"
      width="38px"
      height="38px"
    />
    <div class="name">ようこそ{{ userInfo.name }}さん</div>
    <div class="Timer">
      <p class="date">{{ date }}</p>
      <p class="time">{{ time }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Timer",
  props: {
    userInfo: Object,
  },
  data: function () {
    return {
      date: "",
      time: "",
    };
  },

  mounted: function () {
    setInterval(this.Time, 1000);
  },

  methods: {
    Time: function () {
      let now = new Date();
      this.time =
        now.getHours() + ":" + now.getMinutes() + ":" + now.getSeconds();
      this.date =
        now.getFullYear() + "/" + (now.getMonth() + 1) + "/" + now.getDate();
    },
  },
};
</script>

<style>
.header {
  background-color: #37beb0;
  height: 60px;
  margin-bottom: 30px;
}
.mark {
  float: left;
  font-size: 35px;
  font-family: fantasy;
  color: rgb(243, 126, 100);
  padding-top: 8px;
  padding-left: 15px;
}
.logo {
  float: right;
  border-radius: 50%;
  position: absolute;
  top: 3px;
  right: 40px;
}
.name {
  float: right;
  font-size: 10px;
  padding-top: 44px;
  padding-right: 15px;
  font-family: sans-serif;
}
.Timer {
  float: right;
  font-size: 22px;
  position: absolute;
  right: 220px;
  font-family: Franklin Gothic Medium;
}
.date {
  margin: 3px;
}

.time {
  margin: 3px;
}
</style>