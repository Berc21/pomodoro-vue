<template>
  <div id="app">
    <main class="container">
          <pomodoro/>
    </main>

    <notifications group="pomodoro"  position="top center" />

    <notifications group="settings"  position="bottom center" />

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
@import "https://fonts.googleapis.com/css?family=Ubuntu";


body {
  background: #E6ECF0;
  color: #262C36;
  font-family: 'Ubuntu', sans-serif;
}

strong { font-weight: bold; }
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

/** Notification Style */

.vue-notification {
  padding: 10px;
  margin: 0 5px 5px;

  font-size: 20px;

  color: #33B5E5;
  background: white;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  border: none;

  &.warn {
     color: #FF8800;
     background: white;
     box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  }

  &.error {
    color: #FF3547;
    background: white;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  }

  &.success {
    color: #00C851;
    background: white;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  }
}


</style>


