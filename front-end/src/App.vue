<template>
<div id="app">
  <div class="header">
    <div class="menu">
      <router-link to="/">
      <div class="logo">
        <img class="logo" src="./assets/logo.png" alt="PhotoBank Logo">
      </div>
    </router-link>
    <div class="title">
      <h1>Photo Bank of Free Images</h1>
    </div>
    <div></div>
    <div class="upload-link">
      <a @click="showModal">Upload Pictures</a>
    </div>
    </div>
    
    <modal v-show="isModalVisible" @close="closeModal"></modal>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <a href="https://github.com/helgabalashova/cp4" target="_blank">GitHub</a>
  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  background-image: url(./assets/nature.jpeg);
  height: 60vh;
  background-size: cover;
  background-attachment: fixed;
  background-position: center;
}

.menu {
  background-color: rgba(0, 0, 0, 0.5);
  display: grid;
  grid-template-columns: 1fr 4fr 4fr 4fr;
  padding: 10px 100px 0px 100px;
}

.logo {
  width: 50px;
}

.upload-link {
  margin-top: 22px;
  justify-self: end;
  cursor: pointer;
}

.upload-link a {
  color: white;
  text-decoration: none;
  margin-top: 40px;
}

.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 20px;
  color: white;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 100px 0px 100px;
  background: #e3e3e3;
  font-size: 12px;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>

<script>
import modal from './views/Modal.vue'
export default {
  name: "main",
  components: {
    modal,
  },
  data() {
    return {
      isModalVisible: false,
    }
  },
  methods: {
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    }
  }
}
</script>
