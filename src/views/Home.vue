<template>
  <div class="Home">
    <div class="main">
      <form action="" @submit.prevent="">
        <!--The toParent is function that emits the event parenttochild  -->
        <input
          type="url"
          name="input"
          id="input"
          @keydown="getSource"
          v-model="url"
          @keyup="toParent"
          placeholder="Enter the Url..."
        />
      </form>
      <div class="out">
        <img :src="img_src" alt="" />
      </div>
      <button @click="display" class="code">Get Code</button>
      <div v-if="this.show" class="tags">
        <p>
          <strong>
            Use this image tag below to use the Qr Code in your project:
          </strong>
        </p>
        <p>&lt;img src="{{ img_src }}" alt=""/&gt;</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // This is data used by the Home component
      name: "Home",
      base_url: "https://qrtag.net/api/qr_transparent_6.png",
      url: " ",
      img_src: "",
      values: [],
      show: false
    };
  },
  methods: {
    // This function handles the extraction of data from the API
    getSource(e) {
      if (e.key === "Enter") {
        this.img_src = `${this.base_url}?url=${this.url}`;
        this.values.push(this.url);
      }
    },
    // This function is a emits an event "childtoparent" and send data
    toParent() {
      this.$emit("childtoparent", this.values);
    },
    // This function shows the img tag code for the Qr Code
    display() {
      if (this.show === false) {
        this.show = true;
      } else {
        this.show = false;
      }
    }
  }
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
form input {
  outline: none;
  border: none;
  height: 40px;
  width: 250px;
  background-color: #e4eaf0;
}
.main {
  background-color: white;
  margin-top: 50px;
  margin-left: 330px;
  height: 370px;
  width: 600px;
  box-shadow: rgb(0, 0, 0, 0.75);
  border-radius: 10px;
}
.main form input {
  border-radius: 10px;
  color: black;
  margin-top: 20px;
  margin-left: 160px;
}
.out {
  margin-top: 20px;
}
.out img {
  margin-left: 190px;
}
.code {
  cursor: pointer;
  outline: none;
  border: none;
  background-color: #50a0f0;
  color: white;
  padding: 5px;
  border-radius: 10px;
  margin-right: 5px;
  float: right;
}
.tags {
  margin-top: 30px;
}
.tags p {
  background-color: #e4eaf0;
  margin-top: 5px;
  font-size: 20px;
  width: 90%;
  margin-left: 5%;
}
</style>
