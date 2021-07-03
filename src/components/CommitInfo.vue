<template>
  <div id="commitinfo" v-if="show_flag">
    <h1>総コミット数</h1>
    <h2>{{ table_commit }}</h2>
    <h1>動かせるstep数</h1>
    <h2>{{ commit }}</h2>
    <h1>あと{{ user_node.step - user_step }}ステップ</h1>
    <select v-model="next_node" id="next-node-selector">
      <option disabled value="">次に移動するマスを選択してください</option>
      <option v-for="node in user_node.nextNode" :key="node">{{ node }}</option>
    </select>
    <input v-model="comment" placeholder="ここにコメントを入力してください" />
    <p>
      次は<span>{{ next_node }}</span
      >番のノード
    </p>
    <p>コメント :{{ comment }}</p>
    <input
      type="button"
      value="マスをすすめる"
      @click="moveToNext"
      :disabled="disabledCommit"
    />
  </div>
</template>

<script>
import mapdata from "../assets/mapdata.json";

export default {
  data: function () {
    return {
      all_commit: 0,
      commit: 0,
      table_commit: 0,
      user_node: 0,
      user_step: 0,
      show_flag: false,
      next_node: 0,
      comment: "",
    };
  },
  props: {
    sidebarEvent: Object,
    userInfo: Object,
  },
  watch: {
    userInfo(updatedInfo) {
      this.user_node = mapdata[parseInt(updatedInfo.node_id) - 1];
      this.user_step = updatedInfo.step;
      this.show_flag = true;
      //userのコミット情報を取得する
      this.axios
        .get("http://localhost:3000/api/get-commit")
        .then((res) => {
          console.log(res.data);
          this.all_commit = res.data.all_commit;
          this.commit = res.data.commit;
          this.table_commit = res.data.table_commit;
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
  computed: {
    disabledCommit: function () {
      //１マス進む場合
      if (this.commit - (this.user_node.step - this.user_step) >= 0) {
        if (this.next_node == 0) return true;
        else {
          if (mapdata[parseInt(this.next_node) - 1].type == "checkpoint") {
            if (this.comment == "") return true;
            else return false;
          } else {
            return false;
          }
        }
      } else {
        return false;
      }
    },
  },
  methods: {
    moveToNext() {
      //同じノードでthis.commit分進める場合
      var node = this.user_node.id;
      var commit_count = this.all_commit;
      var step = this.user_step + this.commit;
      //１マス分進む場合
      if (this.commit + this.user_step >= this.user_node.step) {
        commit_count =
          this.table_commit + (this.user_node.step - this.user_step);
        step = 0;
        node = this.next_node;
      }
      this.next_node = 0;
      console.log("aaa");
      console.log(this.user_node.id);
      console.log(node);
      console.log(mapdata[parseInt(node) - 1]);
      if (
        this.user_node.id != node &&
        mapdata[parseInt(node) - 1].type == "checkpoint"
      ) {
        this.axios
          .get(
            "http://localhost:3000/api/visit-checkpoint/" +
              node +
              "/" +
              commit_count +
              "/" +
              this.comment
          )
          .then((res) => {
            console.log(res.data);
          })
          .catch((e) => {
            console.log(e);
          });
      }
      this.axios
        .get(
          "http://localhost:3000/api/update-user/" +
            commit_count +
            "/" +
            node +
            "/" +
            step
        )
        .then((res) => {
          console.log(res.data);
          this.sidebarEvent.moveToNext.userMovingFlag = true;
          this.sidebarEvent.moveToNext.nextNode = mapdata[parseInt(node) - 1];
          this.$emit("moveToNext", this.sidebarEvent);
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
};
</script>

<style>
</style>