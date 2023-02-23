<template>
  <div
      class="event"
      :style="eventPosition">
    {{ event.start }} -
    {{ event.duration }}
  </div>
</template>

<script>
import {parseISO, getMinutes } from "date-fns";

export default {
  name: "VehicleEvent",
  props: {
    event: {
      type: Object,
      required: true
    },
    oneMinuteSize: {
      type: Number,
      required: true
    }
  },
  computed: {
    positionLeft() {
      return getMinutes(parseISO(this.event.start)) || 0
    },
    eventPosition() {
      if (this.event.isWithinStart) {
        return `
        width: ${this.event.duration * this.oneMinuteSize }px;
        left: ${this.positionLeft * this.oneMinuteSize}px;`
      } else {
        return `
        width: ${this.event.duration * this.oneMinuteSize }px;
        left: ${this.positionLeft * this.oneMinuteSize}px;`
      }
    }
  }
}
</script>

<style scoped>
.event{
  height: 40px;
  position: absolute;
  left: 0;
  top: 10px;
  background: lightcoral;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
