<script setup>
import { ref, computed } from 'vue'
import {
  startOfMonth,
  endOfMonth,
  startOfWeek,
  endOfWeek,
  addDays,
  addMonths,
  format
} from 'date-fns'

import DateItem from './DateItem.vue'

const currentDate = ref(new Date())
const currentMonth = ref(currentDate.value)

const days = computed(() => {
  const start = startOfWeek(startOfMonth(currentMonth.value))
  const end = endOfWeek(endOfMonth(currentMonth.value))
  const daysArray = []
  let date = start
  while (date <= end) {
    daysArray.push({
      date: new Date(date),
      isCurrentMonth: date.getMonth() === currentMonth.value.getMonth(),
      reminders: []
    })
    date = addDays(date, 1)
  }
  return daysArray
})

const isActualMonth = computed(() => {
  return currentDate.value.getMonth() !== currentMonth.value.getMonth()
})

const weekdays = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']

const nextMonth = () => {
  currentMonth.value = addMonths(currentMonth.value, 1)
}

const prevMonth = () => {
  currentMonth.value = addMonths(currentMonth.value, -1)
}
</script>

<template>
  <div class="calendar">
    <div class="header">
      <v-btn
        @click="prevMonth"
        :disabled="!isActualMonth"
        icon="mdi-arrow-left-thick"
        variant="plain"
      />
      <h2>{{ format(currentMonth, 'MMMM yyyy') }}</h2>
      <v-btn @click="nextMonth" icon="mdi-arrow-right-thick" variant="plain" />
    </div>
    <div class="calendar-grid">
      <div v-for="day in weekdays" :key="day" class="calendar-item weekday">{{ day }}</div>
    </div>
    <div class="calendar-grid">
      <DateItem v-for="day in days" :key="day.date" :date="day" />
    </div>
  </div>
</template>

<style scoped>
.calendar {
  width: 100%;
  margin: auto;
  padding: 1rem;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
}

.calendar-item {
  display: flex;
  justify-content: center;
  width: 100%;
  border: 1px solid grey;
}

.weekdays {
  display: flex;
  justify-content: space-between;
}

.weekday {
  text-align: center;
  font-weight: bold;
}

.days {
  display: flex;
  flex-wrap: wrap;
}
</style>
