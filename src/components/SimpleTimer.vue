<script setup lang="ts">
import { ref, reactive, computed, onBeforeUnmount, Ref, nextTick } from "vue";

// defineProps<{ msg: string }>();
const timer: Ref<number | null> = ref(null),
  time = reactive({
    hours: 0,
    mins: 0,
    secs: 0,
  }),
  running = computed(() => {
    return !!timer.value;
  });

/** Remaining time in seconds */
let remaining_time = 0;

function timer_callback() {
  remaining_time = time.hours * 60 * 60 + time.mins * 60 + time.secs;

  console.log("remaining", remaining_time);

  if (remaining_time <= 0) {
    stop_timer();
    return;
  }

  remaining_time--;

  let seconds_to_convert = remaining_time;
  let hours, mins;

  hours = Math.floor(seconds_to_convert / 3600);
  if (hours) seconds_to_convert -= hours * 3600;

  mins = Math.floor(seconds_to_convert / 60);
  if (mins) seconds_to_convert -= mins * 60;

  time.secs = seconds_to_convert;
  time.mins = mins;
  time.hours = hours;
}

function on_input(event: Event, period_type: "hours" | "mins" | "secs") {
  event.preventDefault();

  // todo: validation of input
  if (!event || !event.target) return;
  const target = event.target as HTMLDivElement;

  if (!target.textContent) return;
  console.log(target.textContent);

  let value = parseInt(target.textContent);

  if (value >= 60) {
    value = 60;
    console.log("value reduced", value);
  }

  time[period_type] = value || 0;
}

function set_timer() {
  timer.value = setInterval(timer_callback, 1000);
}

function start_timer() {
  console.log("start");
  set_timer();
}

function stop_timer() {
  console.log("stop");

  if (timer.value == null) return;

  clearInterval(timer.value);
  timer.value = null;
}

onBeforeUnmount(() => {
  stop_timer();
});
</script>

<template>
  {{ "running: " + running }}<br />

  <main class="main">
    <div class="timer">
      <label text="Hours input">
        <input
          class="timer__input"
          :disabled="running"
          type="number"
          step="1"
          min="0"
          max="99"
          size="2"
          required
          placeholder="00"
          v-model="time.hours"
        />
      </label>
      :
      <label text="Minutes input">
        <input
          class="timer__input"
          :disabled="running"
          type="number"
          step="1"
          min="0"
          max="60"
          size="2"
          required
          placeholder="00"
          v-model="time.mins"
        />
      </label>
      :
      <label text="Seconds input">
        <input
          class="timer__input"
          :disabled="running"
          type="number"
          step="1"
          min="0"
          max="99"
          size="2"
          required
          placeholder="00"
          v-model="time.secs"
        />
      </label>
    </div>

    <div class="controls">
      <button class="btn" @click="start_timer" :disabled="running">
        Start
      </button>

      <button class="btn" @click="stop_timer" :disabled="!running">Stop</button>
    </div>
  </main>
</template>

<style scoped lang="scss">
.btn {
  cursor: pointer;
  border: 0;

  color: var(--clr-light);
  font-size: 1.5rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;

  background-color: var(--clr-primary);
  padding: 0.4em 1.2em;
  border-radius: 0.2em;

  &:hover:not(:disabled),
  &:focus-visible:not(:disabled) {
    outline: 0.1em solid var(--clr-light);
    outline-offset: 0.1em;
  }

  &:active {
    box-shadow: inset 0 0.1em 0.5em 0.1em black;
    color: #dedede;
    // text-shadow: 1px 1px white, -1px -1px #444;
  }
  &:disabled {
    background-color: grey;
  }
}

.main {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.timer {
  font-size: 3rem;
  margin: 0 auto;
  width: max-content;
  user-select: none;
  display: flex;
  gap: 0.3em;

  &__input {
    font-size: 3rem;
    line-height: 1;
    text-align: center;
    background: 0;
    color: var(--clr-light);
    border: 0;
    border-bottom: 0.1em solid var(--clr-light);
    transition: color 200ms ease-in-out, border-color 200ms ease-in-out;

    &:disabled {
      font-weight: var(--fw--bold);
      border-bottom-color: transparent;
    }

    &:focus-within {
      outline: none;
      color: var(--clr-primary);
      border-bottom-color: var(--clr-primary);
    }

    &::-webkit-inner-spin-button,
    &::-webkit-outer-spin-button {
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
      margin: 0;
    }
  }
}

.controls {
  margin: 0 auto;
  width: max-content;
  display: flex;
  gap: 1rem;
}
</style>
