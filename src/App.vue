<template>
  <div id="app">
    <div class="nav">
      <div class="title"><h1>Qr Code Generator</h1></div>
      <!-- This button controls the toggling  of the history view -->
      <div class="home">
        <button type="" @click="show = false">Home</button>
      </div>
      <div class="his"><button @click="showHistory">History</button></div>
    </div>
    <!--This childtoparent is a emited event that is emitted from the Home Component  fromChild is a function that runs when the data is sent from the home component-->
    <home @childtoparent="fromChild" v-show="!show"> </home>
    <!-- The Sent is a prop for the History Component  and ChildData is the Data sent to the History component-->
    <history v-if="show" :Sent="ChildData"></history>
    <div class="footer"></div>
  </div>
</template>
<script>
import Home from "../src/views/Home.vue";
import History from "../src/views/History.vue";
export default {
  data() {
    return {
      ChildData: [],
      show: false
    };
  },
  components: {
    // This is the local registratin of the components
    Home,
    History
  },
  name: "App",
  methods: {
    //This function receives the data from the Home component into App
    fromChild(value) {
      this.ChildData = value;
      console.log(this.ChildData);
    },
    showHistory() {
      this.show = true;
    }
  }
};
</script>
<style>
#app {
  background-color: #e4eaf0;
  height: 100vh;
}
.nav {
  padding: 5px;
  display: grid;
  grid-template-columns: 3fr 1fr 1fr;
  background-color: white;
}
.title {
  text-align: left;
  color: #3a90e7;
}
.home {
  text-align: right;
  padding-top: 7px;
}
.home button {
  cursor: pointer;
  font-size: 16px;
  border-radius: 15px;
  outline: none;
  border: none;
  padding: 10px;
  color: white;
  background-color: #50a0f0;
  transition: 0.5s;
}
.his {
  text-align: right;
  padding-top: 7px;
  padding-right: 20px;
}
.his button {
  cursor: pointer;
  font-size: 16px;
  border-radius: 15px;
  outline: none;
  border: none;
  padding: 10px;
  color: white;
  background-color: #85c1e9;
  transition: 0.5s;
}
<!-- This styles the hover attributes of the buttons > .home button:hover {
  background-color: #5dade2;
}
.his button {
  background-color: #5dade2;
}

.footer {
  position: absolute;
  height: 30px;
  width: 100%;
  top: 95%;
  background-color: #5dade2;
}
</style>
