<template>
  <div class="cell" ref="itemCell">
    <template v-if="events.length">
      <VehicleEvent
          v-for="event in events"
          :event="event"
          :key="event.id"
          :oneMinuteSize="oneMinuteSize"
      />
    </template>
  </div>
</template>

<script>
import VehicleEvent from "@/components/VehicleEvent.vue";
export default {
  name: "CalendarCell",
  components: { VehicleEvent },
  props: {
    carId: {
      type: Number,
      required: true,
    },
    hour: {
      type: Number,
      required: true,
    },
    events: {
      type: Array,
      required: false,
    },
    gridSize: {
      type: Number,
      required: false,
    },
    dayDuration: {
      type: Number,
      required: false,
    },
  },

  computed: {
    oneMinuteSize() {
      const oneHourSize = this.gridSize / this.dayDuration
      return oneHourSize / 60
    },
    // processedEvents() {
    //   return this.events.map((item) => {
    //     return { ... item, isWithinStart: getHours(parseISO(item.start)) === this.hour }
    //   })
    // }
  }

}
</script>

<style scoped>
.cell {
  height: 60px;
  border-bottom: 1px solid #ccc;
  font-size: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  padding: 4px;
  box-sizing: border-box;
}
</style>
