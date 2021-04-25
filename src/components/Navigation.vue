<template>
  <nav id="nav">
    <div class="nav-brand">
        Covid19 Avail
    </div>
    <!-- <div class="nav-items flex-center">
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
    </div> -->
  </nav>
</template>

<style lang="scss" scoped>
#nav {
  background: #262628 !important;
}
.nav-brand{
    padding: 12px;
    font-size: 16px;
    font-weight: 800;
    color: $primary;
    font-family: 'Roboto Mono', monospace;
}
.nav-items {
  flex-direction: row;
  gap: 12px;
  a {
    align-items: initial;
    padding: 12px;
    font-weight: bold;
    color: #2c3e50;
    text-decoration: none;
    width: max-content;

    &.router-link-exact-active {
      display: block;
      color: #42b983;
    }
    &.router-link-exact-active::after {
      display: block;
      content: " ";
      height: 2px;
      width: 10px;
      background: #42b983;
    }
  }
}
</style>
