<template>

  <div >


  <h1 class="heading">Manage your time wisely</h1>


  <section class="timer-container">

      <main class="timer-main" :class="timerColor">
        <span class="timer-main__countdown">{{ minutes }}:{{ seconds }} </span>
        <span class="timer-main__mode">{{mode}}</span>
      </main>

      <section class="timer-controls">
          <ul class="timer-menu">
            <li class="timer-menu__item">
                 <button v-show="!timer" @click="startTimer" >
                     Start
                 </button>
                  <button v-show="timer" @click="stopTimer">Stop</button>
            </li>
             <li  class="timer-menu__item">
              <button v-show="this.resetButton" @click="resetTimer">Reset</button>
            </li>
          </ul>

      </section>

      <footer class="timer-footer">



       <button class="timer-footer__item" v-show="isPlaying" @click="stopAlarm">Stop Alarm</button>

      </footer>

  </section>


  <section class="timer-stats">
      <h1 class="timer-stats__heading">Stats</h1>

      <p class="timer-stats__pomodoro" > <strong> Completed Work:</strong> {{pomodoroCompleted}}</p>
       <p class="timer-stats__pomodoro" > <strong> Taken Short Break:</strong> {{shortBreakCompleted}} </p>
        <p class="timer-stats__pomodoro" > <strong> Taken Long Break:</strong> {{longBreakCompleted}} </p>
  </section>


  <section class="timer-settings">

    <h1>Settings</h1>

  <div class="timer-settings__time">
    <label for="work">Work Time <hr ><span style="text-align:center;display: inherit;margin-top: 5px;">{{calculateWorkTime}} </span></label>
    <input name="work" type="text" v-model="vWorkMin" placeholder="minute">
    <input name="work" type="text" v-model="vWorkSec"  placeholder="second">
    <button @click="setDefaultWorkTime">Change</button>
  </div>

  <hr class="timer-settings__hr">

   <div class="timer-settings__time">
    <label for="short-break">Short Break <hr ><span style="text-align:center;display: inherit;margin-top: 5px;">{{calculateShortBreakTime}} </span></label>
    <input name="short-break" type="text" v-model="vShortBreakMin" placeholder="minute">
    <input name="short-break" type="text" v-model="vShortBreakSec"  placeholder="second">
    <button @click="setDefaultShortBreak" >Change</button>
  </div>


   <hr class="timer-settings__hr">

  <div class="timer-settings__time">
    <label for="long-break">Long Break  <hr ><span style="text-align:center;display: inherit;margin-top: 5px;">{{calculateLongBreakTime}} </span> </label>
    <input name="long-break" type="text" v-model="vLongBreakMin" placeholder="minute">
    <input name="long-break" type="text" v-model="vLongBreakSec"  placeholder="second">
    <button @click="setDefaultLongBreak">Change</button>
  </div>

    <hr class="timer-settings__hr">

   <div class="timer-settings__time">
    <label for="long-break-lap">Long Break Lap  <hr ><span style="text-align:center;display: inherit;margin-top: 5px;">{{whenLongBreak}} </span>  </label>
    <input name="long-break-lap" type="text" v-model="vwhenLongBreak" placeholder="lap">
    <button @click="setWhenLongBreak">Change</button>
  </div>

  </section>

    </div>
</template>

<script>
export default {
  name: "Pomodoro",
  data() {
    return {
      totalTime: 5,
      defaultWorkTime: 5,
      defaultLongBreakTime: 3,
      defaultShortBreakTime: 2,
      whenLongBreak: 4,
      vwhenLongBreak: null,
      vWorkMin: null,
      vWorkSec: null,
      vLongBreakMin: null,
      vLongBreakSec: null,
      vShortBreakMin: null,
      vShortBreakSec: null,
      timer: null,
      resetButton: false,
      pomodoroCompleted: 0,
      longBreakCompleted: 0,
      shortBreakCompleted: 0,
      mode: "Work",
      pomodoroTime: true,
      lastTwoMode: [],
      alarm: new Audio(
        "http://soundbible.com/mp3/Fire_pager-jason-1283464858.mp3"
      ),
      isPlaying: false
    };
  },
  methods: {
    startTimer() {
      this.timer = setInterval(() => this.countdown(), 1000);
      this.resetButton = true;
      this.stopAlarm();
    },
    stopTimer() {
      clearInterval(this.timer);
      this.timer = null;
    },
    resetTimer() {
      clearInterval(this.timer);
      this.resetButton = false;
      this.timer = null;
      this.totalTime = this.defaultWorkTime;
    },
    countdown() {
      this.totalTime -= 1;
    },
    paddNumber(num) {
      num = String(num);
      return (
        (num < 10 && num.match(/^[0][a-zA-Z]{13}/) ? "0" : "") + num.toString()
      );
    },
    playAlarm() {
      this.alarm.loop = true;
      this.alarm.play();

      this.isPlaying = !this.alarm.paused;
    },
    stopAlarm() {
      if (this.alarm) {
        this.alarm.pause();
        this.alarm.currentTime = 0;
        this.isPlaying = false;
      }
    },
    settingsValidation() {
      this.$notify({
        group: "settings",
        title: "Invalid input",
        text: "Please write only number!",
        type: "error"
      });
    },
    setDefaultWorkTime() {
      if (this.vWorkMin == 0 && this.vWorkSec == 0) return;
      if (!(this.vWorkMin && this.vWorkSec)) return;

      let min = Number(this.vWorkMin);
      let sec = Number(this.vWorkSec);

      if (!(Number.isInteger(min) && Number.isInteger(sec))) {
        this.vWorkMin = null;
        this.vWorkSec = null;
        this.settingsValidation();
        return;
      }

      this.defaultWorkTime = min * 60 + sec;

      this.totalTime = min * 60 + sec;

      this.$notify({
        group: "settings",
        title: "Success!",
        text:
          "Long break time is " +
          this.paddNumber(this.vWorkMin) +
          ":" +
          this.paddNumber(this.vWorkSec),
        type: "success"
      });

      this.vWorkMin = null;
      this.vWorkSec = null;
    },
    setDefaultLongBreak() {
      if (this.vLongBreakMin == 0 && this.vLongBreakMin == 0) return;
      if (!(this.vLongBreakMin && this.vLongBreakMin)) return;

      let min = Number(this.vLongBreakMin);
      let sec = Number(this.vLongBreakSec);

      if (!(Number.isInteger(min) && Number.isInteger(sec))) {
        this.vLongBreakMin = null;
        this.vLongBreakSec = null;
        this.settingsValidation();
        return;
      }

      this.defaultLongBreakTime = min * 60 + sec;

      this.$notify({
        group: "settings",
        title: "Success!",
        text:
          "Long break time is " +
          this.paddNumber(this.vLongBreakMin) +
          ":" +
          this.paddNumber(this.vLongBreakSec),
        type: "success"
      });

      this.vLongBreakMin = null;
      this.vLongBreakSec = null;
    },
    setDefaultShortBreak() {
      if (this.vShortBreakMin == 0 && this.vShortBreakSec == 0) return;
      if (!(this.vShortBreakMin && this.vShortBreakSec)) return;

      let min = Number(this.vShortBreakMin);
      let sec = Number(this.vShortBreakSec);

      if (!(Number.isInteger(min) && Number.isInteger(sec))) {
        this.vShortBreakMin = null;
        this.vShortBreakSec = null;
        this.settingsValidation();
        return;
      }

      this.defaultShortBreakTime = min * 60 + sec;

      this.$notify({
        group: "settings",
        title: "Success!",
        text:
          "Short break time is " +
          this.paddNumber(this.vShortBreakMin) +
          ":" +
          this.paddNumber(this.vShortBreakSec),
        type: "success"
      });

      this.vShortBreakMin = null;
      this.vShortBreakSec = null;
    },
    setWhenLongBreak() {
      if (this.vwhenLongBreak == 0) return;
      if (!this.vwhenLongBreak) return;

      this.whenLongBreak = this.vwhenLongBreak;

      this.vwhenLongBreak = null;

      this.$notify({
        group: "settings",
        title: "Success!",
        text: "Long break lap is " + this.whenLongBreak,
        type: "success"
      });
    }
  },
  computed: {
    minutes() {
      let min = Math.floor(this.totalTime / 60);
      return this.paddNumber(min);
    },
    seconds() {
      let sec = this.totalTime % 60;
      return this.paddNumber(sec);
    },
    timerColor() {
      if (this.timeforLongBreak) {
        return "green";
      }
      if (this.timeforShortBreak) {
        return "orange";
      }
    },
    calculateWorkTime() {
      let sec = this.defaultWorkTime % 60;

      let min = Math.floor(this.totalTime / 60);

      return `${this.paddNumber(min)}:${this.paddNumber(sec)}`;
    },
    calculateShortBreakTime() {
      let sec = this.defaultShortBreakTime % 60;

      let min = Math.floor(this.defaultShortBreakTime / 60);

      return `${this.paddNumber(min)}:${this.paddNumber(sec)}`;
    },
    calculateLongBreakTime() {
      let sec = this.defaultLongBreakTime % 60;

      let min = Math.floor(this.defaultLongBreakTime / 60);

      return `${this.paddNumber(min)}:${this.paddNumber(sec)}`;
    },
    timeforLongBreak() {
      if (this.pomodoroCompleted == 0) return false;

      return (
        (this.pomodoroCompleted % this.whenLongBreak == 0 ? true : false) &
        !this.pomodoroTime
      );
    },
    timeforShortBreak() {
      return !this.timeforLongBreak && !this.pomodoroTime;
    },
    keepModeHistory() {
      this.lastTwoMode.push(this.mode);

      if (this.lastTwoMode.length > 2) {
        this.lastTwoMode.splice(0, 1);
      }

      return this.lastTwoMode;
    }
  },
  watch: {
    totalTime(n) {
      if (n == 0) {
        this.playAlarm();
        this.resetTimer();
        if (this.pomodoroTime) {
          this.pomodoroCompleted++;
        }

        this.pomodoroTime = !this.pomodoroTime;
        this.mode = "Work";

        if (this.timeforShortBreak) {
          this.mode = "Short Break";
          this.totalTime = this.defaultShortBreakTime;

          this.$notify({
            group: "pomodoro",
            title: "Time for a short break!",
            text: "You're working hard, have a rest!",
            type: "warn"
          });
        }

        if (this.timeforLongBreak) {
          this.mode = "Long Break";
          this.totalTime = this.defaultLongBreakTime;

          this.$notify({
            group: "pomodoro",
            title: "Time for a long break!",
            text: "You're working hard, have a rest!",
            type: "success"
          });
        }

        if (this.mode == "Work") {
          this.$notify({
            group: "pomodoro",
            title: "Work Time",
            text: "Start getting things done!"
          });
        }

        switch (this.keepModeHistory[0]) {
          case "Long Break":
            this.longBreakCompleted++;
            break;
          case "Short Break":
            this.shortBreakCompleted++;
            break;
        }
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.heading {
  font-size: 24px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 20px;
}

.green {
  background: #4caf50 !important;
}
.orange {
  background: #ff5722 !important;
}

.timer-container {
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  padding: 30px;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  background: #fff;
  border-radius: 3px;
}

// Main
.timer-main {
  position: relative;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  flex-flow: column wrap;
  max-width: 224px;
  height: 224px;
  border: 4px solid #fff;
  border-radius: 50%;
  background: #272e38;

  &::before,
  &::after {
    content: "";
    display: block;
    position: absolute;
    top: 50%;
    left: 50%;
    border: 1px solid #dce4e7;
    border-radius: 50%;
    transform: translate(-50%, -50%);
  }
  &::before {
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
    transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
  }

  &:hover::before {
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
  }

  &::before {
    width: 120%;
    height: 120%;
  }

  &::after {
    width: 80%;
    height: 80%;
  }
  &__mode {
    font-weight: 500;
    font-size: 30px;
    text-align: center;
    color: #dce4e7;
  }
  &__countdown {
    text-align: center;
    color: #dce4e7;
    font-size: 60px;
  }
}

.timer-controls {
  width: 50%;
  margin: 50px auto 0 auto;
}

.timer-menu {
  display: flex;
  justify-content: space-between;
}

.timer-footer {
  margin-top: 20px;
  display: flex;
  justify-content: space-around;
  &__item {
    text-align: center;
  }
}

.timer-stats {
  background: #fff;
  max-width: 500px;
  width: 100%;
  padding: 30px;
  margin: 40px auto;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  border-radius: 3px;
  &__heading {
    text-align: center;
    font-size: 32px;
    margin-bottom: 10px;
  }
  &__pomodoro {
    margin-bottom: 10px;
  }
}

.timer-settings {
  max-width: 500px;
  width: 100%;
  background: #fff;
  padding: 30px;
  margin: 40px auto;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  border-radius: 3px;
  h1 {
    font-size: 32px;
    text-align: center;
    margin-bottom: 30px;
  }

  &__time {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 20px 0;

    input {
      max-width: 50px;
      height: 30px;
      border-radius: 5px;
      text-align: center;
      border: none;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
    }
  }

  &__hr {
    border-color: #272e38;
  }
}
</style>

