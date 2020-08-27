<template>
  <html>
  <head>
    <meta charset="UTF-8">
  </head>
  <body>

    <div class="hello">
    <nuxt-link to="../../">ホーム画面に戻る</nuxt-link><br>

      <h1>{{title2}}</h1>
      <p>{{message2}}</p>

      <!-- <div v-for="item in items"><hr/> -->
      <div v-for="(item, num) in items" :key="num"><hr/>
        <h2>{{item.contents.title}}へのアクセス</h2>
        <p>所要時間：{{item.time}}</p>
        <p v-html="item.access"></p>
      </div>
    </div>

    <nuxt-link to="../../">ホーム画面に戻る</nuxt-link><br>
    <!-- <hr/><hr/>
    <div>{{items}}</div> -->

    <br><br><hr/><hr/>
    <h1>microCMS</h1>       
    <hr/><hr/>
    <!-- <div>{{items}}</div> -->
    
  </body>
  </html>
</template>




<script>
  const axios = require('axios'); // axios利用

  export default {
    data: function() {    
      return{
        title2: 'アクセス',
        // message2:'観光地のアクセス表示: 画像クリックによるリンク(GoogleMapアクセス詳細)',
        message2:'観光地のアクセス表示',
      }    
    },

    // methods:{ },

    // <!-- microCMS　APIキー--> 
    async asyncData () {
      const { data } = await axios.get('https://listservice.microcms.io/api/v1/access', {
        headers: { 'X-API-KEY': '2ab80902-8ca9-432d-b832-29927c3a2aa9' }
      })
      return {
        items: data.contents, 
        // items: data.contents, //これでJSON形式でデータを持ってこれるらしい
      }
    },

  }

</script>





<style scoped>
  .hello {
    background-color: #99FFCC;
    color: #000000;
  }
  .container {
    padding: 5px 10px;
  }
  h1 {
    font-size: 60pt;
    color: #345980;
  }
  p {
    padding-top:5px;
    font-size: 20pt;
  }
  div {
      font-size:14pt;
  }
  pre {
    padding: 10px;
    font-size: 18pt;
    background-color: #efefef;
    white-space: pre-wrap ;
  }
  hr {
    margin:10px 0px;
  }
  button {
    width: 500px;
    padding: 5px;
    font-size: 18pt;
    background-color: #efefef;

  }
</style>