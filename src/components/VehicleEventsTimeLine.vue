<template>
  <div class="line">
    <template v-if="events.length">
      <VehicleEvent
          v-for="event in events"
          :key="event.id"
          :event="event"
          :oneMinuteSize="oneMinuteSize"
      />
    </template>
  </div>
</template>

<script>
import VehicleEvent from "@/components/VehicleEvent.vue";
export default {
  name: "VehicleEventsTimeLine",
  components: { VehicleEvent },
  props: {
    events: {
      type: Array,
    },
    oneMinuteSize: {
      type: Number,
    },
  }
}
</script>

<style lang="scss" scoped>
.line{
  height: 60px;
  position: relative;
  border-bottom: 1px solid #ccc;
  box-sizing: border-box;
}
</style>
