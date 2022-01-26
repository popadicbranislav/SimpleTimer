<script setup lang="ts">
import { ref, reactive, computed, onBeforeUnmount, Ref } from "vue";

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

function on_input(event: any, period_type: "hours" | "mins" | "secs") {
  // todo: validation of input
  const value = parseInt(event.target.innerText);
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
  {{ "timer: " + timer }}
  <main class="main">
    <div class="timer">
      <span :contenteditable="!running" @input="on_input($event, 'hours')">{{
        time.hours
      }}</span>
      :
      <span :contenteditable="!running" @input="on_input($event, 'mins')">{{
        time.mins
      }}</span>
      :
      <span :contenteditable="!running" @input="on_input($event, 'secs')">{{
        time.secs
      }}</span>
    </div>

    <div class="controls">
      <button class="btn" @click="start_timer" :disabled="running">
        start
      </button>
      <button class="btn" @click="stop_timer" :disabled="!running">stop</button>
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

  &:hover,
  &:focus-visible {
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

  // & > *::selection {
  //   color: green;
  //   background-color: red;
  // }

  & > * {
    transition: border-color 200ms ease-in-out;
    transition: color 200ms ease-in-out;
    border-bottom: 0.1em solid transparent;
  }

  & > *[contenteditable="true"] {
    border-bottom-color: var(--clr-light);
  }

  & > *:focus-within {
    outline: none;
    color: var(--clr-primary);
    border-bottom-color: var(--clr-primary);
  }
}

.controls {
  margin: 0 auto;
  width: max-content;
  display: flex;
  gap: 1rem;
}
</style>
