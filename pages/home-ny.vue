<template>
<html>
  <head>
    <meta charset="UTF-8">
  </head>
  <body>
    <div class="hello">
      <h1>{{title2}}</h1>
      <p>{{message2}}</p>
      <hr>
      <p>{{message3}}</p>
      <!-- microCMSでのコンテンツ自動追加用のfor文 -->
      <div v-for="(item, index) in items" v-bind:key="index">
        <hr>
        <!-- ボタン表示 -->
        <button class="button" v-on:click="item.isShow = !item.isShow">
          {{item.title}} 掲載日：{{item.date}}
          <p v-html="item.body"></p>
        </button>
        <button class="subButton" v-on:click="doAll(true)">全表示</button>
        <button class="subButton" v-on:click="doAll(false)">全非表示</button>
        <!-- リンク[アクセス] -->
        <nuxt-link to="/access/_id/access_page">アクセス</nuxt-link>
        <!-- 個別表示 -->
        <transition>
          <div class="panel content" v-show="item.isShow">
            <p>[概要]</p>
            {{item.textarea}}
            <br>
            <br>
          </div>
        </transition>
      </div>
    </div>
  </body>
</html>
</template>
<script>
export default {
  data() {
    return {
      items: [],
      // itemShowState: new Set(),
      title2: "観光地の紹介",
      // message2: "39: Set利用　整えた版",
      message3: "クリックにより詳細表示"
    };
  },
  methods: {
    doAll(newState) {
      for (let item of this.items) {
        this.$set(item, "isShow", newState); // $set(vue特有のメソッド)
      }
    }
  },
  // <!-- microCMS　APIキー-->
  async asyncData({ app, params }) {
    const { data } = await app.$axios
      .get("https://listservice.microcms.io/api/v1/pic", {
        headers: { "X-API-KEY": "2ab80902-8ca9-432d-b832-29927c3a2aa9" }
      })
      .catch(err => {
        console.log(`err: ${err}`);
      });
    let items = data.contents;
    for (let item of items) {
      item.isShow = false;
    }
    return {
      items: items
      //      items: data.contents, //これでJSON形式でデータを持ってこれるらしい
    };
  }
};
</script>
<style>
.hello {
  background-color: #CCFFFF;
  color: #345980;
}
.container {
  padding: 5px 10px;
}
h1 {
  font-size: 60pt;
  color: #345980;
}
p {
  padding-top: 5px;
  font-size: 20pt;
}
div {
  font-size: 14pt;
}
pre {
  padding: 10px;
  font-size: 18pt;
  background-color: #efefef;
  white-space: pre-wrap;
}
hr {
  margin: 10px 0px;
}
button {
  width: 500px;
  padding: 5px;
  font-size: 18pt;
  background-color: #CCFFFF;
  border: 1px solid #000;
  border-color: #000000;
}
.subButton {
  width: 60px;
  height: 25px;
  padding: 0px;
  font-size: 10pt;
  color: #ffffff;
  background-color: #345980;
  border: 1px solid #000;
  border-color: #000000;
}
/* 本vue.js p190 */
.panel {
  border: 1px solid #000;
  width: 500px;
  overflow: hidden;
}
/* アニメーション全体の設定 */
.v-enter-active,
.v-leave-active {
  transition: height 1s;
}
/* アニメーション前後のスタイルを設定 */
.v-enter {
  height: 0px;
}
.v-enter-to {
  height: 500px;
}
.v-leave {
  height: 500px;
}
.v-leave-to {
  height: 0px;
}
</style>