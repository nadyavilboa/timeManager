<template>
  <div class="time-manager">
    <header class="timer-header">
      <div class="timer-name">Таймер</div>
      <button class="timer-toggle-wrapper">
        <span
          class="toggle-item"
          :class="{active: isFirst}"
          >
            Спорт
          </span>
        <span
          class="toggle-item"
          :class="{active: !isFirst}"
          >
            Завтрак
        </span>
      </button>
    </header>
    <div class="timer-wrapper">
      <div class="input-wrapper">
        <label class="label" for="timer-input">Введите время в минутах</label>
        <input class="input" type="number" id="timer-input" name="timer-input" />
      </div>
      <div class="timer-value">14:10:01</div>
    </div>
    <div class="buttons-wrapper">
      <button class="btn" disabled>Старт</button>
      <button class="btn" disabled>Пауза</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TimeManager',
  data() {
    return {
      isFirst: true,
    }
  }
}
</script>

<style scoped>
  .time-manager {
    display: flex;
    flex-direction: column;
    gap: 20px;
    width: fit-content;
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

  .buttons-wrapper {
    display: flex;
    gap: 10px;
  }
</style>
