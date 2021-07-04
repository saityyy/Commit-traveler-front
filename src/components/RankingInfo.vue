<template>
  <div id="rankinginfo">
    <p>{{ rankingInfo }}</p>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      rankingInfo: {},
    };
  },
  created() {
    //マップデータベース更新
    //this.axios
    //.post("http://localhost:3000/api/fetch-ranking")
    //.then((res) => {
    //console.log(res.data);
    //this.rankingInfo = res.data;
    //})
    //.catch((e) => {
    //console.log(e);
    //});
  },
};
</script>

<style>
</style>