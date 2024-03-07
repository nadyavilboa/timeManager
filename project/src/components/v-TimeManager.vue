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
          :disabled="isInputDisabled"
          ref="inputElement"
          @change="inputChange($event.target.value)"
          @keyup.enter="enterKeyHandler"
        />
      </div>
      <div class="timers-wrapper">
        <v-Timer
          v-for="timer in timers"
          :key="timer.id"
          v-show="timer.isShow"
          :value="timer.value"
          :isWork="timer.isWork"
          :isPaused="timer.isPaused"
          :isContinue="timer.isContinue"
        />
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
      <button
        class="btn"
        :disabled="disabledPause"
        @click="stopTimer"
      >
        {{ pauseOrContinue ? 'Продолжить' : 'Пауза' }}
      </button>
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
      timers: [
        {
          id: 0,
          name: 'Спорт',
          value: 0,
          isShow: true,
          isStartDisabled: true,
          isPaused: false,
          isContinue: false,
          isWork: false,
        },
        {
          id: 1,
          name: 'Завтрак',
          value: 0,
          isShow: false,
          isStartDisabled: true,
          isPaused: false,
          isContinue: false,
          isWork: false,
        }
      ],
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
      activeTimer.isStartDisabled = !activeTimer.isWork;
      return activeTimer ? activeTimer.isWork : false;
    },
    isInputDisabled() {
      const activeTimer = this.getActiveTimer;
      return activeTimer.isWork || activeTimer.isPaused;
    },
    disabledPause() {
      const activeTimer = this.getActiveTimer;
      return activeTimer ? !activeTimer.isWork : true;
    },
    pauseOrContinue() {
      const activeTimer = this.getActiveTimer;
      return activeTimer ? activeTimer.isPaused : false;
    }
  },
  methods: {
    // переключение между таймерами
    toggleTimer() {
      this.timers.forEach(timer => {
        if (timer) {
          timer.isShow = !timer.isShow;
        }
      })

      this.$refs.inputElement.value = '';

    },

    // принимаем целые положительные числа не начинающиеся с нуля, без знаков
    inputChange(value) {
      const inputVal = value;
      let inputNumbers = '';

      for (let i = 0; i < inputVal.length; i++) {
        const currentChar = inputVal[i];

        if (['+', '-', '.'].includes(currentChar)) {
          continue;
        }

        if (!Number.isInteger(Number(currentChar))) {
            continue;
        }
        
        inputNumbers = inputNumbers.concat(currentChar);
      }

      // проверяем, что получилось число, делаем его положительным
      if (!isNaN(inputNumbers)) {
        inputNumbers = Number(Math.abs(inputNumbers));
      }

      this.$refs.inputElement.value = String(inputNumbers);

      const activeTimer = this.getActiveTimer;
      if (activeTimer) {
        activeTimer.value = Number(inputNumbers);
        activeTimer.isStartDisabled = false;
      }
    },

    // запуск таймера
    workTimer() {
      const activeTimer = this.getActiveTimer;
      if (activeTimer) {
        activeTimer.isWork = !activeTimer.isWork;
      }
      
      if (this.$refs.inputElement) {
        this.$refs.inputElement.value = '';
      }

      //состояние кнопки старт
      activeTimer.isStartDisabled = activeTimer.isWork;

      // сброс кнопки Пауза, если таймер сброшен
      activeTimer.isPaused = !activeTimer.isWork ? false : activeTimer.isPaused;
    },
    enterKeyHandler() {
      this.$refs.inputElement.value ? this.workTimer() : '';
    },
    stopTimer() {
      const activeTimer = this.getActiveTimer;
      activeTimer.isPaused = !activeTimer.isPaused;

      if (!activeTimer.isPaused) {
        activeTimer.isWork = true;
        activeTimer.isContinue = true;
      }
    }
  },
  mounted() {
    // нужно обрабатывать нажатие для всего документа
    document.addEventListener( 'keyup', event => {
      if( event.code === 'Enter' ) this.enterKeyHandler();
    });
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

  .input-wrapper .input:disabled {
    opacity: 0.3;
    cursor: not-allowed;
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
