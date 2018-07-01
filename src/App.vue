<template>
  <div id="app">
    <main class="container">
          <pomodoro/>
    </main>

  </div>
</template>

<script>
import Pomodoro from "@/components/Pomodoro";

export default {
  name: "App",
  components: {
    Pomodoro
  }
};
</script>

<style lang="scss">
@import "reset-css";

button {
  border: none;
  margin: 0;
  padding: 0;
  width: auto;
  overflow: visible;

  background: transparent;

  /* inherit font & color from ancestor */
  color: inherit;
  font: inherit;

  /* Normalize `line-height`. Cannot be changed from `normal` in Firefox 4+. */
  line-height: normal;

  /* Corrects font smoothing for webkit */
  -webkit-font-smoothing: inherit;
  -moz-osx-font-smoothing: inherit;

  /* Corrects inability to style clickable `input` types in iOS */
  -webkit-appearance: none;
}

.container {
  margin: 100px auto;
  width: 960px;
}
</style>


