<template>
  <div class="toast-warning">
    If you want to contribute to this project here's the
    <a href="">👩‍💻Source code</a>
  </div>
  <footer class="flex-center">
    <p>Made by <a href="https://github.com/5tupidbrain">5tupidBrain</a></p>
    <a href="https://github.com/5tupidbrain/Covid19-avail"
      ><img src="@/assets/img/github-logo.svg" alt=""
    /></a>
  </footer>
</template>

<style lang="scss" scoped>
footer {
  background: #262628;
  padding: 18px;
  font-size: 14px;
  flex-direction: row;
  font-family: "Roboto Mono", monospace;
  justify-content: space-between;
  p {
    margin: 0;
    color: #fff;
  }

  p a {
    color: $primary;
  }

  img {
    height:24px;
    filter: invert(1);
  }
}
.toast-warning {
  padding: 12px 18px;
  width: 100%;
  text-align: center;
  font-family: "Roboto Mono", monospace;
  font-size: 14px;
  font-weight: bold;
  background: rgba($primary, 0.5);

  a {
    color: #ff0000;
  }
}
</style>
