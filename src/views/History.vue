<template>
  <div class="history">
    <div v-if="!check" class="list">
      <ul>
        <!-- Here data is the prop passed from the App Component-->
        <li v-for="(data, index) in Data" :key="index">
          <div class="text">{{ data }}</div>
          <div class="dlt">
            <i
              class="fa fa-times"
              aria-hidden="true"
              @click="removeHistory(data)"
            ></i>
          </div>
        </li>
      </ul>
      <div class="clear">
        <button @click="DeleteHistory">Delete History</button>
      </div>
    </div>
    <div v-if="check" class="empty">
      <h1>We have nothing to show right now</h1>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "History",
      Data: []
    };
  },
  props: {
    Sent: {
      type: Array
    }
  },
  // This is a life cycle hook(trigers before Component is rendered) that loads the data from the local storage
  beforeMount() {
    this.loadHistory();
    this.Data.push(...this.Sent);
    this.Sent.splice(0, this.Sent.length);
  },
  // This is a life cycle hook(trigers berfore the Componenr is removed from the DOM) it saves the history pf the QR code
  beforeDestroy() {
    this.saveHistory();
  },
  computed: {
    // This checks whether the data sent by parent is empty
    check() {
      return this.Sent.length < 1 && this.Data.length < 1;
    }
  },
  methods: {
    // This Function handles the local storage
    loadHistory() {
      if (localStorage.getItem("Data")) {
        try {
          this.Data = JSON.parse(localStorage.getItem("Data"));
        } catch (e) {
          localStorage.removeItem("Data");
        }
      }
    },
    // This function removes the History item from the array and local storage
    removeHistory(x) {
      let Index = this.Data.indexOf(x);
      this.Data.splice(Index, 1);
      this.saveHistory();
    },
    // This function saves the data into the local storage
    saveHistory() {
      const parsed = JSON.stringify(this.Data);
      localStorage.setItem("Data", parsed);
    },
    // This function deletes all the history from the array and local Storage
    DeleteHistory() {
      this.Data = [];
      localStorage.clear();
    }
  }
};
</script>

<style>
.list {
  overflow-y: scroll;
  height: 500px;
  width: 800px;
  margin-top: 50px;
  margin-left: 250px;
  background-color: white;
  transition: 0.5s;
}
.list ul {
  padding: 10px;
  margin-top: 10px;
}
.list li {
  font-size: 16px;
  display: grid;
  grid-template-columns: 3fr 1fr;
  padding-top: 15px;
  border-radius: 10px;
  width: 770px;
  list-style: none;
  background-color: #e4eaf0;
  text-align: center;
  height: 50px;
  margin-top: 10px;
}

.dlt i {
  font-size: 18px;
  border-radius: 5px;
  color: red;
  cursor: pointer;
  transition: 0.5s;
}
.dlt i:hover {
  font-size: 14px;
}
.text {
  text-align: center;
}

.empty {
  margin-top: 20px;
  margin-left: 400px;
}

.clear button {
  border: none;
  outline: none;
  padding: 5px;
  float: right;
  margin-right: 7px;
  background-color: #50a0f0;
  color: white;
  border-radius: 10px;
  cursor: pointer;
}
</style>
