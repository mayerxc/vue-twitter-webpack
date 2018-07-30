<template>
  <div id="app">
    <input v-model="search" type="text">
    <button v-on:click="getTweets(search)">Search!</button>
    <div v-if="showTweets" style="display:flex; flex-direction: row; align-items: center;justify-content: center;flex-wrap:wrap;">
      <div v-for="tweet in tweetArr" >
        <Tweet 
        :id="tweet.id_str"
          :options="{width:'255', align:'center'}" 
        ></Tweet>
      </div>
    </div>


    <h1>Error tweet:</h1>
    <Tweet :id="'14'" error-message="This tweet could not be loaded" error-message-class="tweet--not-found"></Tweet>
    <h1>With Computer:</h1>
    <div style="width:400px;"><Tweet :id="'1022636072608694272'" :options="{width:'255', align:'center'}"></Tweet></div>
    <!-- <Tweet :id="'1022636072608694272'" :options="{width:'255', align:'center'}"></Tweet> -->
    <h1>without computer</h1>
    <Tweet :id="'1022636072608694272'" :options="{'cards': 'hidden'}"></Tweet>
    
    <img src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <br />
    <AnotherComponent msg="what the hell is going on?"/>
    <Tweet :id="'783943172057694208'"></Tweet>
  </div>
</template>

<script>
import axios from 'axios';
// import cors from 'cors'
import Tweet from 'vue-tweet-embed/tweet';
import AnotherComponent from './components/AnotherComponent';
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'app',
  components: {
    HelloWorld,
    AnotherComponent,
    Tweet
  },
  data: function(){
    return {
      search: '',
      showTweets: false,
      tweetArr: [],
    }
  },
  methods: {
    getTweets: function() {
      self = this;
      let url = `https://twitter-backend.herokuapp.com/query?search=${this.search}`;
      // let url = `http://localhost:3000/query?search=${this.search}`
      // let url = `http://api.icndb.com/jokes/random/10`
      axios.get(url).then(function(response){
        console.log(response);
        self.tweetArr = response.data.statuses
        self.showTweets = true;
      })
    }
  }
} 
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
Tweet {
  /* display: block; */
  float:right;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
  text-align: center;
}

</style>
