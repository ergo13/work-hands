<template>
  <div class="calendar">
    <div class="calendar__moth-swap">
      <span @click="prevMonth" class="calendar__moth-swapper"> &#x2190; </span>
      {{ currentMonthName }}
      {{ currentYear }}
      <span @click="nextMonth" class="calendar__moth-swapper"> &#x2192; </span>
    </div>

    <ul class="calendar__week-days">
      <li v-for="week in weekDaysName" :key="`week-${week}`">
        {{ week }}
      </li>
    </ul>

    <ul class="calendar__month-days">
      <li
        v-for="(day, index) in daysInCurrentMonth"
        :class="[
          'calendar__day',
          { 'calendar__active-day': dateCompare(day.date) },
        ]"
        :key="`day-${index}`"
        @click="selectDay(day)"
      >
        {{ day.dayInt }}
      </li>
    </ul>
    <br />
    <div>Выбранный день: {{ getSelectedDay }}</div>
    <br />
    <select name="languages" id="languages-select" @change="languageChange">
      <option value="">Выбор языка</option>
      <option value="ru-RU">Русский</option>
      <option value="en-EN">Английский</option>
    </select>
  </div>
</template>
<script>
export default {
  props: {
    propDate: {
      type: String,
      required: false,
    },
  },

  data() {
    return {
      localDate: new Date(new Date().setHours(0, 0, 0, 0)),

      selectedDay: new Date(new Date().setHours(0, 0, 0, 0)),

      formatterConfig: {
        weekday: {
          weekday: "short",
        },

        month: {
          month: "long",
        },

        date: {
          month: "long",
          year: "numeric",
          day: "numeric",
        },
      },

      currentLocal: "ru-RU",
    };
  },

  computed: {
    weekdayFormatter() {
      return new Intl.DateTimeFormat(
        this.currentLocal,
        this.formatterConfig.weekday
      );
    },

    monthFormatter() {
      return new Intl.DateTimeFormat(
        this.currentLocal,
        this.formatterConfig.month
      );
    },

    dateFormatter() {
      return new Intl.DateTimeFormat(
        this.currentLocal,
        this.formatterConfig.date
      );
    },

    getSelectedDay() {
      return this.dateFormatter.format(this.selectedDay);
    },

    currentMonthInt() {
      return this.localDate.getMonth();
    },

    currentMonthName() {
      return this.monthFormatter.format(this.localDate);
    },

    currentYear() {
      return this.localDate.getFullYear();
    },

    weekDaysName() {
      const days = [];
      const monday = new Date(2025, 7, 18);
      for (let i = 0; i < 7; i++) {
        const day = new Date(monday).setDate(monday.getDate() + i);
        days.push(this.weekdayFormatter.format(new Date(day)));
      }
      return days;
    },

    qwe() {
      return this.selectedDay.getTime();
    },

    daysInCurrentMonth() {
      const date = new Date(this.localDate);
      const monthStart = new Date(date.setDate(1));
      const dayInMonth = new Date(
        this.currentYear,
        this.currentMonthInt + 1,
        0
      ).getDate();
      const days = new Array(monthStart.getDay() - 1).fill({});

      for (let i = 0; i < dayInMonth; i++) {
        const date = new Date(
          this.currentYear,
          this.currentMonthInt,
          monthStart.getDate() + i
        ).setHours(0, 0, 0, 0);
        const dayInt = i + 1;
        days.push({ date: new Date(date), dayInt });
      }

      return days;
    },
  },

  methods: {
    languageChange(event) {
      const language = event.target.value;
      this.currentLocal = language;
    },

    dateCompare(date) {
      if (date instanceof Date) {
        return date.getTime() === this.selectedDay.getTime();
      }

      return false;
    },

    selectDay(day) {
      this.selectedDay = day.date;
    },

    nextMonth() {
      this.localDate = new Date(this.currentYear, this.currentMonthInt + 1);
    },

    prevMonth() {
      this.localDate = new Date(this.currentYear, this.currentMonthInt - 1);
    },
  },

  created() {
    if (this.propDate) {
      const date = new Date(new Date(this.propDate).setHours(0, 0, 0, 0));
      this.localDate = date;
      this.selectedDay = date;
    }
  },
};
</script>

<style scoped lang="css">
.calendar {
  max-width: 600px;
  border: 1px solid rgba(0, 0, 0, 0.1);
  padding: 10px;
}

.calendar__moth-swapper {
  cursor: pointer;
}

.calendar__week-days,
.calendar__month-days {
  list-style: none;
  padding: 0;
  margin: 0;
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.calendar__day {
  padding: 2px;
  border: 1px solid transparent;
  width: fit-content;
}

.calendar__active-day {
  border: 1px solid blue;
}

.calendar__moth-swap {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
</style>
