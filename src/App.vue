<template>
  <div id="app">
    <h1>Error tweet:</h1>
    <Tweet :id="'14'" error-message="This tweet could not be loaded" error-message-class="tweet--not-found"></Tweet>
    <h1>With Computer:</h1>
    <Tweet :id="'1022636072608694272'"></Tweet>
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
};
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
