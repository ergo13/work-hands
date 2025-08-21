<template>
  <div id="app">
    <div style="display: flex; justify-content: center; column-gap: 40px;">
      <div>
        без пропса
        <Calendar />
      </div>

      <div>
        с пропсом
        <Calendar propDate="2015-05-13" />
      </div>
    </div>
  </div>
</template>

<script>
import Calendar from "./components/Calendar.vue";

export default {
  name: "App",
  components: {
    Calendar,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
</style>
