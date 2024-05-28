<script setup>
defineProps({
  date: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div class="date-item">
    <div class="header-date" :class="{ 'not-current-month': !date.isCurrentMonth }">
      {{ date.date.getDate() }}
    </div>
    <div class="reminders">
      <span class="reminder" v-for="reminder of date.reminders" :key="reminder">
        {{ reminder.name }}
      </span>
    </div>
  </div>
</template>

<style scoped>
.date-item {
  width: 100%;
  height: 100px;
  display: flex;
  flex-direction: column;
  border: 1px solid grey;
}

.header-date {
  display: flex;
  justify-content: flex-end;
}

.reminders {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.not-current-month {
  color: #ccc;
}
</style>
