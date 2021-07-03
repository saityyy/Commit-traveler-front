<template>
  <div class="header">
    <div class="mark">codeGo</div>
    <div class="userInfo">
      <img
        class="logo"
        :src="
          'https://avatars.githubusercontent.com/' + userInfo.name + '?s=500'
        "
        width="45px"
        height="45px"
      />
      <div class="name">ようこそ{{ userInfo.name }}さん</div>
    </div>
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
  height: 80px;
  margin-bottom: 30px;
}
.mark {
  float: left;
  font-size: 35px;
  font-family: fantasy;
  color: rgb(243, 126, 100);
  padding-top: 20px;
  padding-left: 20px;
}
.userInfo {
  float: right;
  position:absolute;
  top:5px;
  right:30px;
}
.logo {
  border-radius: 50%;
}
.name {
  font-size: 14px;
  font-family:sans-serif;
  font-weight:bold;
}
.Timer {
  float: right;
  font-size: 25px;
  position: absolute;
  right: 250px;
  font-family: Franklin Gothic Medium;
}
.date {
  margin: 8px;
}

.time {
  margin: 5px;
}
</style>