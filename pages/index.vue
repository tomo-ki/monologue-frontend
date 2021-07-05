<template>
  <div id="app">
    <h1>独り言App</h1>
    <p>つぶやく内容を入力してください</p>
    <div>
      <input type="text" name="tweet" id="tweet" v-model="newTweet">
      <button @click="insertTweet">つぶやく</button>
    </div>
    <h2>つぶやき一覧</h2>
    <table class="table">
      <tr v-for="tweet in tweetLists" :key="tweet.id">
        <td class="tweet__redcircle-area"><div class="tweet__redcircle"></div></td>
        <td class="tweet__content">{{tweet.content}}</td>
        <td>
          <button @click="deleteTweet(tweet.id)">削除</button>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTweet: "",
      tweetLists: [],
    };
  },
  methods: {
    async getTweet() {
      const resData = await this.$axios.get(
        "http://127.0.0.1:8000/api/tweet/"
      );
      this.tweetLists = resData.data.data;
    },
    async insertTweet() {
      const sendData = {
        content: this.newTweet,
      };
      await this.$axios.post(
        "http://127.0.0.1:8000/api/tweet/", sendData);
      this.newTweet = "";
      this.getTweet();
    },
    async deleteTweet(id) {
      await this.$axios.delete(
        "http://127.0.0.1:8000/api/tweet/" + id);
      this.getTweet();
    },
  },
  created(){
    this.getTweet();
  },
};
</script>

<style>
#app{
  display: flex;
  flex-direction: column;
  align-items: center;
}
.table {
  border: solid 1px #AAAAAA;
  padding: 10px;
}
tr{
  height: 50px;
}
.tweet__redcircle{
  background-color:red;
  width: 40px;
  height: 40px;
  border-radius: 50%;
}
.tweet__content{
  font-size: 12px;
  width: 400px;
  padding: 0 5px;
}
</style>
