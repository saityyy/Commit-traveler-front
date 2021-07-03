<template>
  <div id="userinfo">
    <div class="center">
      <img
        class="logo"
        :src="
          'https://avatars.githubusercontent.com/' + userInfo.name + '?s=500'
        "
        width="150px"
        height="150px"
      />
      <h2>{{ userInfo.name }}</h2>
      <div>
        <h3>累計コミット数</h3>
        <h4>{{ all_commit }}</h4>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Timer",
  data: function () {
    return {
      all_commit: 0,
    };
  },
  props: {
    userInfo: Object,
  },
  watch: {
    userInfo(v) {
      console.log(v);
      this.axios
        .get("http://localhost:3000/api/get-commit")
        .then((res) => {
          this.all_commit = res.data.all_commit;
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
};
</script>

<style>
.center {
  text-align: center;
}
</style>