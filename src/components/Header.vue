<template>
  <div class="header">
    <div class="mark">codeGo</div>
    <img src="../assets/icon.png" width="20px" height="20px" />
    <div class="name">ようこそ？？？さん</div>
    <div id="Timer">
      <p>{{ date }}</p>
      <p>{{ time }}</p>
    </div>
  </div>
</template>

<script>
export default {
  el: "#Timer",
  data: function () {
    return {
      date: "",
      time: "",
    };
  },

  mounted: function () {
    setInterval(this.Time, 1000);
  },

  methods:{
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
</style>