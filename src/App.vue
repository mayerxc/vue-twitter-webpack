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
          <md-checkbox v-model="searchLocation" v-on:change="getLocation()">Search by Location</md-checkbox>
          <md-content v-if="searchLocation">{{`${city}, ${region}`}}</md-content>
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
              <md-toolbar class="md-layout-item md-size-70 md-small-size-95 md-xsmall-size-100 md-layout md-alignment-top-center">
                <md-field>
                  <label>Search Twitter Users</label>
                  <md-input v-on:keyup.enter="getUsers()" v-model="searchUser" type="text"></md-input>
                  <md-button v-on:click="getUsers()">
                    <md-icon >search</md-icon>
                  </md-button>
                </md-field>
              </md-toolbar>
            </div>

            <div class="md-layout md-alignment-top-center">
              <md-content class="md-layout-item md-size-10 md-small-size-40 md-xsmall-size-90">
                <md-field >
                  <label>Number of Users</label>
                  <md-input v-model="tweetUserNumber" type="number">{{tweetUserNumber}}</md-input>
                </md-field>
              </md-content>
            </div>

            <div class="md-layout md-alignment-top-center">
              <md-card class="md-layout-item md-size-70 md-small-size-95 md-xsmall-size-100">
                <md-content>
                  <md-progress-spinner 
                    v-if="showSpinnerUser"
                    :md-diameter="100" 
                    :md-stroke="10" 
                    md-mode="indeterminate"
                  >
                  </md-progress-spinner>
                  <div class="md-layout md-alignment-top-center" v-if="showUsers">
                    <div class="md-layout-item" v-for="user in userArr">
                      <timeline 
                        :id="user.screen_name" 
                        :sourceType="'profile'" 
                        :options="{ tweetLimit: '3', width: '255', align: 'center' }" 
                        error-message="This user could not be loaded" 
                        error-message-class="user--not-found" 
                      >
                        <md-progress-spinner :md-diameter="30" :md-stroke="3" md-mode="indeterminate"></md-progress-spinner>
                      </timeline>
                    </div>
                  </div>
                </md-content>
              </md-card>
            </div>
          </div>
        </md-tab>
      </md-tabs>
    </div>
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
// import func from './vue-temp/vue-editor-bridge';

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
      tweetSearchNumber: 10,
      tweetUserNumber: 5,
      lat: 45.52345,
      lon: -122.67621,
      city: null,
      region:null,
      radius: 20,
      showTweets: false,
      showUsers: false,
      showSpinner: false,
      showSpinnerUser: false,
      searchLocation: false,
      tweetArr: [],
      userArr: [],
    }
  },

  methods: {
    getTweets: function() {
      let self = this;
      
      //get spinner going
      self.showSpinner = true;

      //clear old tweets
      self.showTweets = false;
      self.tweetArr = [];
      
      //set URL
      let url = `https://twitter-backend.herokuapp.com/query`;
      
      function runAxios(url, params){
        axios.get(url, params).then(function(response){
          console.log(response);
        
          //stop spinner
          self.showSpinner = false;
          
          //load and show tweets
          self.tweetArr = response.data.statuses
          self.showTweets = true;
        }).catch(function(error){
          console.log(error);
        })
      }
      
      //set up params
      let params = {};
      if (self.searchLocation === true){
        console.log("searchLocation true?", self.searchLocation);
        params = {
          "params": {
            "search": self.search,
            "geocode": `${self.lat},${self.lon},${self.radius}mi`,
            "count": self.tweetSearchNumber
          }
        }
        runAxios(url, params);
      
      } else {
        params = {"params": {"search": self.search, "count":self.tweetSearchNumber } }
        runAxios(url, params);
      }
      console.log("params are:", params)
      
      
    },

    getUsers: function () {
      let self = this;
      self.showSpinnerUser = true;
      self.userArr = [];
      self.showUsers = false;
      let url = `https://twitter-backend.herokuapp.com/user?user=${self.searchUser}&count=${self.tweetUserNumber}`;
      // let url = `http://localhost:3000/user?user=${this.searchUser}`;
      axios.get(url)
        .then( function(response) {
          console.log(response);
          self.showSpinnerUser = false;
          self.userArr = response.data
          self.showUsers = true;
        })
        .catch((error) => {
          console.log("Error in searching for twitter user");
          console.log(error);
        })
    },

    getLocation: function(){
      let self = this;
      let url = `http://ip-api.com/json`
      axios.get(url)
        .then(function(response){
          console.log("response from ip-api:", response)
          self.lat = response.data.lat;
          self.lon = response.data.lon;
          self.city = response.data.city;
          self.region = response.data.region;
          console.log("lat:", self.lat);
          console.log("lon:", self.lon);

          let data = response.data;
          console.log("data from ip-api", data);
        })
        .catch(function(error) {
          console.log("error from ip api", error);
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
  float:right;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
  text-align: center;
}



</style>
