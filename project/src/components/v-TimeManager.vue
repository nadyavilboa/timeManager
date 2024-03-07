<template>
  <div class="time-manager">
    <header class="timer-header">
      <div class="timer-name">Таймер</div>
      <button
        class="timer-toggle-wrapper"
        @click="toggleTimer"
      >
        <span
          class="toggle-item"
          :class="{active: timer.isShow}" 
          v-for="timer in timers"
          :key="timer.id"
        >
          {{ timer.name }}
        </span>
      </button>
    </header>
    <div class="timer-wrapper">
      <div class="input-wrapper">
        <label
          class="label"
          for="timer-input"
        >
          Введите время в минутах
        </label>
        <input
          class="input"
          type="number"
          id="timer-input"
          name="timer-input"
          @change="inputChange($event.target.value)"
        />
      </div>
      <div class="timers-wrapper">
        <v-Timer v-for="timer in timers" :key="timer.id" v-show="timer.isShow" :value="timer.value" :isWork="timer.isWork" />
      </div>
    </div>
    <div class="buttons-wrapper">
      <button
        class="btn"
        :disabled="disabledStart"
        @click="workTimer"
      >
        {{ startOrStop ? 'Сброс' : 'Старт' }}
      </button>
      <button class="btn" disabled>Пауза</button>
    </div>
  </div>
</template>

<script>
import vTimer from './v-Timer.vue';

export default {
  name: 'v-TimeManager',
  components: {
    vTimer,
  },
  data() {
    return {
      isFirst: true,
      timers: [
        {
          id: 0,
          name: 'Спорт',
          value: 0,
          isShow: true,
          isStartDisabled: true,
          isPaused: false,
          isWork: false,
        },
        {
          id: 1,
          name: 'Завтрак',
          value: 0,
          isShow: false,
          isStartDisabled: true,
          isPaused: false,
          isWork: false,
        }
      ],
      inputElement: null,
    }
  },
  computed: {
    getActiveTimer() {
      return this.timers.find(timer => timer.isShow)
    },
    disabledStart() {
      const activeTimer = this.getActiveTimer;
      return activeTimer ? activeTimer.isStartDisabled : true;
    },
    startOrStop() {
      const activeTimer = this.getActiveTimer;
      activeTimer.isStartDisabled = !activeTimer.isWork
      return activeTimer ? activeTimer.isWork : false;
    },
  },
  methods: {
    toggleTimer() {
      // переключение таймера
      this.timers.forEach(timer => {
        if (timer) {
          timer.isShow = !timer.isShow;
        }
      })

      this.inputElement.value = '';

    },
    inputChange(value) {
      const activeTimer = this.getActiveTimer;
      if (activeTimer) {
        activeTimer.value = Number(value);
        activeTimer.isStartDisabled = false;
      }
    },
    workTimer() {
      const activeTimer = this.getActiveTimer;
      if (activeTimer) {
        activeTimer.isWork = !activeTimer.isWork;
      }
      
      if (this.inputElement) {
        this.inputElement.value = '';
      }
    }
  },
  mounted() {
    this.inputElement = document.querySelector('#timer-input');
  }
}
</script>

<style scoped>
  .time-manager {
    display: flex;
    flex-direction: column;
    gap: 20px;
    width: fit-content;
    min-width: 300px;
    margin: 0 auto;
  }

  .timer-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
  }

  .timer-toggle-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 8px;
    border: 2px solid #048a81;
    background: transparent;
    padding: 0;
  }

  .toggle-item {
    padding: 8px 10px;
  }

  .toggle-item.active {
    background: #048a81;
    color: #fff;
    border-radius: 4px;
  }

  .timer-name {
    font-size: 32px;
    font-weight: 500;
    line-height: 120%;
  }

  .timer-wrapper {
    display: flex;
    align-items: flex-end;
    gap: 10px;
  }

  .input-wrapper {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 4px;
  }

  .input-wrapper .input {
    padding: 8px 10px;
    border: 2px solid #048a81;
    border-radius: 6px;
  }

  .buttons-wrapper {
    display: flex;
    gap: 10px;
  }

  .btn {
    padding: 8px 10px;
    background: #fff;
    border: 2px solid #048a81;
    color: #048a81;
    border-radius: 6px;
  }

  .btn:disabled {
    opacity: 0.3;
    cursor: not-allowed;
  }
</style>
