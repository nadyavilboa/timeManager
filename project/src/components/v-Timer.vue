<template>
    <div class="timer-val" v-if="isEndTimer && isWork">
      Готово
    </div>
    <div class="timer-val" v-else-if="value && isWork">
      {{ this.getCurrentTime(this.currentTime) }}
    </div>
    <div class="timer-val" v-else>
      hh:mm:ss
    </div>
</template>

<script>
  export default {
    name: 'v-Timer',
    props: {
      value: {
        type: Number,
        default() {
          return 0
        }
      },
      isWork: {
        type: Boolean,
        default() {
          return false
        }
      }
    },
    data() {
      return {
        AMOUNT_TIMER_SYMBOL: 2,
        SECONDS_IN_HOUR: 3600,
        SECONDS_IN_MINUTE: 60,
        TIMER_STEP: 1000,
        currentTime: 0,
        isEndTimer: false,
      }
    },
    methods: {
      formatTimerVal(val) {
        return val.length < this.AMOUNT_TIMER_SYMBOL ? '0' + val : val;
      },
      getCurrentTime(seconds) {
        let hh = String(Math.floor(seconds / this.SECONDS_IN_HOUR));
        let mm = String(Math.floor((seconds % this.SECONDS_IN_HOUR) / this.SECONDS_IN_MINUTE));
        let ss = String(Math.floor(seconds % this.SECONDS_IN_MINUTE));
  
        return this.formatTimerVal(hh) + ':' + this.formatTimerVal(mm) + ':' + this.formatTimerVal(ss);
      },
      animateTimer() {
        console.log(this.currentTime);
        let timerId = setInterval(() => {
          if (this.currentTime === 0) {
            clearInterval(timerId);
            this.isEndTimer = true;
            return;
          }
          this.currentTime--;
        }, this.TIMER_STEP);
      }
    },
    watch: {
      isWork: function(newVal) {
        if (this.value && newVal) {
          this.currentTime = Number(this.value) * this.SECONDS_IN_MINUTE;
          this.animateTimer();
        }
      }
    },
    mounted() {
      console.log('mounted');
      console.log(this.currentTime);
    }
  }
</script>

<style scoped>
</style>