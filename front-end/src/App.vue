<template>
<div id="app">
  <div class="header">
<router-link to="/">
    <div class="title">
      <h1>Party Bestiary</h1>
    </div>
    </router-link>
    <router-link to="/">
      <div class="logo">
        <img src="/ordinary.png" class="titleIcon">
      </div>
    </router-link>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <router-link to="/admin">Monster Editor</router-link>
    <p>Github: <a href="https://github.com/neilsduncan/CS260-CreativeP4.git"> here</a></p>
    <p>Copyrighted content used for educational purposes.</p>
  </div>
</div>
</template>

<style>

.logo{
  padding-left: 16px;
}

html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  color: #cdcdcd;
  background-image: url("/abstract-ancient-antique-art-235985.jpg");
  padding: 0px;
  margin: 0px;
}

.titleIcon {
  width: 60px;
  height: 64px;
}

/* Header */
.header {
  display: flex;
  padding: 10px 100px 0px 100px;
  background-color: #b2b0b2;
  color: #232323;
}

.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 30px;
  color: #232323;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  color: #030303;
  height: 75px;
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
