<template>
  <b-container id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </b-container>
</template>

<style lang="scss">
@import './styles/style.scss';
</style>
