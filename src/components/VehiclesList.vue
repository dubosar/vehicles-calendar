<template>
  <div>
    <div>name</div>
    <div style="border-bottom: 1px solid #ccc;">name</div>
    <div class="vehicle" v-for="vehicle in vehicles" :key="vehicle.id">
      {{ vehicle.model }}
    </div>
  </div>
</template>

<script>
export default {
  name: "VehiclesList",
  props: {
    vehicles: {
      type: Array,
    }
  }
}
</script>

<style scoped>
.vehicle{
  height: 60px;
  border-bottom: 1px solid #ccc;
  box-sizing: border-box;
  display: flex;
  align-items: center;
}
</style>
