<template>
  <div id="app">
    <div>
      <md-tabs md-alignment="centered">
        <md-tab md-label="Search Tweets">
          <h1 class="md-title">Search Tweets</h1>
          <div class="md-layout md-alignment-top-center">
            <md-toolbar class="md-layout-item md-size-70 md-small-size-95 md-xsmall-size-100 md-layout md-alignment-top-center">
      
              <md-field>
                <label>Search Term</label>
                <md-input v-on:keyup.enter="getTweets()" v-model="search" type="text"></md-input>
                <md-button v-on:click="getTweets()">
                  <md-icon >search</md-icon>
                </md-button>
                
              </md-field>
            </md-toolbar>
          </div>
          <md-checkbox v-model="searchLocation">Search by Location</md-checkbox>
          <div class="md-layout md-alignment-top-center">
            <md-content class="md-layout-item md-size-10 md-small-size-40 md-xsmall-size-90">
              <md-field >
                <label>Number of tweets</label>
                <md-input v-model="tweetSearchNumber" type="number">{{tweetSearchNumber}}</md-input>
              </md-field>
            </md-content>
          </div>
          <div class="md-layout md-alignment-top-center">
            <md-card class="md-layout-item md-size-70 md-small-size-95 md-xsmall-size-100">
              <md-content>
                <md-progress-spinner 
                  v-if="showSpinner"
                  :md-diameter="100" 
                  :md-stroke="10" 
                  md-mode="indeterminate"
                >
                </md-progress-spinner>
                <div class="md-layout md-alignment-top-center" v-if="showTweets">
                  <div class="md-layout-item" v-for="tweet in tweetArr" >
                    <Tweet 
                      :id="tweet.id_str"
                      :options="{width:'275', align:'center'}" 
                    >
                      <md-progress-spinner :md-diameter="30" :md-stroke="3" md-mode="indeterminate"></md-progress-spinner>
                    </Tweet>
                  </div>
                </div>
              </md-content>
            </md-card>
          </div>
          
        </md-tab>
        <md-tab  md-label="Search for Twitter Users">
          <div>
            <h1 class="md-title">Search Users</h1>
            <div class="md-layout md-alignment-top-center">
              <md-toolbar class="md-layout-item md-size-60 md-small-size-75 md-xsmall-size-100 md-layout md-alignment-top-center">
                <md-field>
                  <label>Search Twitter Users</label>
                  <md-input v-on:keyup.enter="getUsers()" v-model="searchUser" type="text"></md-input>
                  <md-button v-on:click="getUsers()">
                    <md-icon >search</md-icon>
                  </md-button>
                </md-field>
              </md-toolbar>
            </div>

            <md-content class="md-layout md-alignment-top-center md-size-60">
              <div class="md-layout md-alignment-top-center" v-if="showUsers">
                <div class="md-layout-item" v-for="user in userArr">
                  <timeline 
                    :id="user.screen_name" 
                    :sourceType="'profile'" 
                    :options="{ tweetLimit: '3', width: '255', align: 'center' }" 
                    error-message="This user could not be loaded" 
                    error-message-class="user--not-found" 
                  >
                    <md-progress-spinner :md-diameter="100" :md-stroke="10" md-mode="indeterminate"></md-progress-spinner>
                  </timeline>
                </div>
              </div>
            </md-content>
          </div>
        </md-tab>
      </md-tabs>
      
        
    </div>
    
    <!-- style="display:flex; flex-direction: row; align-items:flex-start; justify-content:space-around;flex-wrap:wrap;" -->
    
    
  </div>
</template>

<script>
import axios from 'axios';
import Tweet from 'vue-tweet-embed/tweet';
import Timeline from 'vue-tweet-embed/timeline';
import AnotherComponent from './components/AnotherComponent';
import HelloWorld from './components/HelloWorld.vue';


import Vue from 'vue'
import VueMaterial from 'vue-material'
import 'vue-material/dist/vue-material.min.css'

Vue.use(VueMaterial)

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
      tweetSearchNumber: 5,
      showTweets: false,
      showUsers: false,
      showSpinner: false,
      searchLocation: false,
      tweetArr: [],
      userArr: [],
    }
  },

  methods: {
    getTweets: function() {
      let self = this;
      self.showSpinner = true;
      self.showTweets = false;
      self.tweetArr = [];
      let url = `https://twitter-backend.herokuapp.com/query?search=${this.search}`;
      // let url = `http://localhost:3000/query?search=${this.search}`
      axios.get(url).then(function(response){
        console.log(response);
        self.showSpinner = false;
        self.tweetArr = response.data.statuses
        self.showTweets = true;
      })
    },

    getUsers: function () {
      let self = this;
      self.showSpinner = true;
      self.userArr = [];
      self.showUsers = false;
      let url = `https://twitter-backend.herokuapp.com/user?user=${this.searchUser}`;
      // let url = `http://localhost:3000/user?user=${this.searchUser}`;
      axios.get(url).then( function(response) {
        console.log(response);
        self.showSpinner = false;
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
  margin-top: 5px;
}
Tweet {
  /* display: block; */
  float:right;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
  text-align: center;
}



/* .md-content {
  width: 300px;
  height: 200px;
  display: inline-flex;
  justify-content: space-around;
  align-items: flex-start;
  } */


</style>
