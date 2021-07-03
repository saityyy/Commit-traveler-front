<template>
  <div id="commitinfo" v-if="show_flag">
    <h2>
      動かせるstep数 : <span>{{ commit }}</span>
    </h2>
    <h2>
      次のノードに行くまであと<span>{{ user_node.step - user_step }}</span
      >ステップ
    </h2>
    <div
      id="select_next_node"
      v-if="this.commit - (this.user_node.step - this.user_step) >= 0"
    >
      <h3>次に進むノードを選択してください</h3>
      <select v-model="next_node" id="next-node-selector">
        <option disabled value="">次に移動するマスを選択してください</option>
        <option v-for="node in user_node.nextNode" :key="node" :value="node">
          {{ node }}
        </option>
      </select>
    </div>
    <div id="comment" v-if="next_node_type == 'checkpoint'">
      <h3>コメントを入力してください</h3>
      <input v-model="comment" placeholder="ここにコメントを入力してください" />
    </div>
    <div id="lang" v-if="next_node_type == 'checkpoint'">
      <h3>推しの言語を入力してください</h3>
      <select v-model="programming_language">
        <option disabled value="">プログラミング言語</option>
        <option v-for="lang in programming_language_list" :key="lang">
          {{ lang }}
        </option>
      </select>
    </div>
    <div id="check" v-if="next_node != 0">
      <h3>下記の内容でマップを進めます</h3>
      <p v-if="this.commit - (this.user_node.step - this.user_step) >= 0">
        ノード{{ this.user_node.id }} => ノード{{ this.next_node }}
      </p>
      <p v-else>step{{ this.user_step }} => step{{ this.commit }}</p>
      <p v-if="next_node_type == 'checkpoint'">コメント :{{ comment }}</p>
      <p v-if="next_node_type == 'checkpoint'">
        推しの言語 :{{ programming_language }}
      </p>
    </div>
    <input type="button" @click="update_next_node" id="button" />
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
      next_node_type: "node",
      comment: "",
      programming_language_list: [],
      programming_language: "",
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
      this.axios
        .get("http://localhost:3000/api/get-langs")
        .then((res) => {
          console.log("get-langs");
          console.log(res.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },
    next_node(updatedInfo) {
      if (updatedInfo > 0)
        this.next_node_type = mapdata[parseInt(updatedInfo) - 1].type;
    },
  },
  computed: {
    disabledCommit: function () {
      //１マス進む場合
      console.log(this.next_node);
      if (this.commit - (this.user_node.step - this.user_step) >= 0) {
        if (this.next_node == 0) return true;
        else {
          if (mapdata[parseInt(this.next_node) - 1].type == "checkpoint") {
            if (this.comment == "") return true;
            else if (this.programming_language == "") return true;
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
    update_next_node() {
      this.next_node = document.getElementById("button").getAttribute("value");
    },
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
#button {
  display: none;
}
</style>