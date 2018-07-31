<template>
  <div id="app">
    <h1>Search Tweets</h1>
    <input placeholder="Search Term" v-model="search" type="text">
    <button v-on:click="getTweets()">Search!</button>
    
    <div v-if="showTweets" style="display:flex; flex-direction: row; align-items:flex-start; justify-content:space-around;flex-wrap:wrap;">
      <div v-for="tweet in tweetArr" >
        <Tweet 
        :id="tweet.id_str"
          :options="{width:'255', align:'center'}" 
        ></Tweet>
      </div>
    </div>
    <div>
      <h1>Search Users</h1>
      <input placeholder="Search Twitter User" type="text" v-model="searchUser">
      <button v-on:click="getUsers()">Search!</button>
      <!-- <Timeline :id="'realDonaldTrump'" :sourceType="'profile'" :options="{ tweetLimit: '3', width: '255', align: 'center' }" error-message="This user could not be loaded" error-message-class="user--not-found" ></Timeline> -->
      <div v-if="showUsers" style="display:flex; flex-direction: row; align-items:flex-start; justify-content:space-around;flex-wrap:wrap;">
      <div v-for="user in userArr" >
        <Timeline 
          :id="user.screen_name" 
          :sourceType="'profile'" 
          :options="{ tweetLimit: '3', width: '255', align: 'center' }" 
          error-message="This user could not be loaded" 
          error-message-class="user--not-found" >
        </Timeline>
      </div>
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
import Timeline from 'vue-tweet-embed/timeline';
import AnotherComponent from './components/AnotherComponent';
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'app',
  components: {
    HelloWorld,
    AnotherComponent,
    Tweet,
    Timeline
  },
  data: function(){
    return {
      search: "",
      searchUser: "",
      showTweets: false,
      showUsers: false,
      tweetArr: [],
      userArr: [],
    }
  },

  methods: {
    getTweets: function() {
      let self = this;
      self.showTweets = false;
      self.tweetArr = [];
      let url = `https://twitter-backend.herokuapp.com/query?search=${this.search}`;
      // let url = `http://localhost:3000/query?search=${this.search}`
      // let url = `http://api.icndb.com/jokes/random/10`
      axios.get(url).then(function(response){
        console.log(response);
        self.tweetArr = response.data.statuses
        self.showTweets = true;
      })
    },

    getUsers: function () {
      let self = this;
      self.userArr = [];
      self.showUsers = false;
      let url = `https://twitter-backend.herokuapp.com/user?user=${this.searchUser}`;
      // let url = `http://localhost:3000/user?user=${this.searchUser}`;
      axios.get(url).then( function(response) {
        console.log(response)
        self.userArr = response.data
        self.showUsers = true;
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
