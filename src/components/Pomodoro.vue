<template>

  <div>


  <h1 class="heading">Manage Your Time with Pomodoro App</h1>


  <section class="timer-container">

      <main class="timer-main" :class="timerColor">
        <span class="timer-main__countdown">{{ minutes }}:{{ seconds }} </span>
        <span class="timer-main__mode">{{mode}}</span>
      </main>

      <footer class="timer-footer">
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

      </footer>

       <p v-show="pomodoroCompleted > 0"> You completed {{pomodoroCompleted}} </p>

       <p v-show="timeforLongBreak == true "> Time for long break </p>
       <p v-show="timeforShortBreak == true"> Time for short break </p>

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
      timer: null,
      resetButton: false,
      pomodoroCompleted: 0,
      mode: "Work",
      pomodoroTime: true
    };
  },
  methods: {
    startTimer() {
      this.timer = setInterval(() => this.countdown(), 1000);
      this.resetButton = true;
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
    paddNumber(num){
      return (num < 10 ? '0' : '') + num.toString();
    },
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
        return 'green'
      }
      if (this.timeforShortBreak) {
        return 'orange'
      }
    },
    timeforLongBreak() {
      if (this.pomodoroCompleted == 0) return false;

      return (
        (this.pomodoroCompleted % 4 == 0 ? true : false) & !this.pomodoroTime
      );
    },
    timeforShortBreak() {
      return !this.timeforLongBreak && !this.pomodoroTime;
    }
  },
  watch: {
    totalTime(n) {
      if (n <= 0) {
        this.resetTimer();
        if (this.pomodoroTime) {
          this.pomodoroCompleted++;
        }

        this.pomodoroTime = !this.pomodoroTime;
        this.mode = "Work";

        if (this.timeforShortBreak) {
          this.mode = "Short Break";
          this.totalTime = this.defaultShortBreakTime;
        }

        if (this.timeforLongBreak) {
          this.mode = "Long Break";
          this.totalTime = this.defaultLongBreakTime;
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
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
    transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
  }

  &:hover::before,
  &:hover::after {
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

.timer-footer {
  width: 50%;
  margin: 50px auto 0 auto;
}

.timer-menu {
  display: flex;
  justify-content: space-between;
}
</style>

