<template>
  <div id="layout">
    <div id="filters">Filters</div>
    <div id="grid" :style="gridTemplateColumn">
      <div class="cars">
        <div>name</div>
        <div>name</div>
        <div class="car d-flex" v-for="car in cars" :key="car.id">
          {{ car.model }}
        </div>
      </div>

      <div class="slider" @mousedown.left="mouseDownListener"></div>

      <div class="calendar scrollable-layout" ref="calendar">
        <div>{{ currentDate }}</div>
        <div class="hours">
          <div v-for="hour in hours" :key="hour" class="hour layout-segment">
            <div class="hour">{{ hour }}</div>
            <CalendarCell
                v-for="car in cars"
                :key="`${car.id}_${hour}`"
                :carId="car.id"
                :hour="hour"
                :dayDuration="hours.length"
                :gridSize="gridSize"
                :events="thisCarEvents(car.id, hour)"
            />
          </div>
        </div>
      </div>

    </div>

  </div>
</template>

<script>

// import vehiclesService from '@/services/vehiclesService'
import {format, parseISO, addMinutes, isWithinInterval, getHours} from 'date-fns'
import CalendarCell from "@/components/CalendarCell.vue";

export default {
  name: 'VehiclesCalendar',
  components: { CalendarCell },
  data: () => ({
    date: new Date(),
    hours: [3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16],
    // hours: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23],
    cars: [
      { id: 11, model: 'BMW', make: '328' },
      // { id: 12, model: 'VW', make: 'Polo' },
      // { id: 13, model: 'Mini', make: 'Countryman' },
      // { id: 14, model: 'Mini', make: 'Cooper' },
      // { id: 15, model: 'Audi', make: 'A3' },
    ],
    events: [
      { id: 1, carId: 11, start: '2023-02-23 01:00:00', duration: 300 },
      // { id: 2, carId: 12, start: '2023-02-23 02:22:00', duration: 90 },
      // { id: 3, carId: 15, start: '2023-02-23 03:00:00', duration: 540 },
      // { id: 4, carId: 11, start: '2023-02-23 04:45:00', duration: 90 },
    ],
    spacerSize: 180,
    gridSize: 0,
    moving: false,
  }),
  mounted () {
    this.gridSize = this.$refs.calendar.scrollWidth
    // this.fetch()
    //     .then()
    //     .catch((error) => console.log(error))
    //     .finally(() => {
    //       this.loadingPage = false
    //     })
  },
  computed: {
    gridTemplateColumn() {
      return `grid-template-columns: ${this.spacerSize}px 5px 1fr`
    },
    currentDate() {
      return format(this.date, "yyyy-MM-dd")
    }
  },
  methods: {
    // fetch () {
    //   return vehiclesService
    //       .load()
    //       .then((resp) => this.cars = resp.data)
    //       .catch((error) => console.log(error))
    // },
    mouseDownListener () {
      this.moving = true
      window.addEventListener('mousemove', this.mouseMove)
      window.addEventListener('mouseup', this.mouseUpListener)
    },
    mouseMove (e) {
      if (!this.moving) return
      this.spacerSize = e.layerX
      this.gridSize = this.$refs.calendar.scrollWidth
    },
    mouseUpListener () {
      if (!this.moving) return
      this.moving = false
    },
    // thisCarEvents(carId, hour) {
    //   const start = addMinutes(parseISO(this.currentDate), (hour * 60))
    //   const end = addMinutes(parseISO(this.currentDate), (hour * 60 + 59))
    //   return this.events.filter((event) => {
    //     return event.carId === carId && isWithinInterval(parseISO(event.start), { start, end })
    //   }) || []
    // },
    thisCarEvents(carId, hour) {
      const start = addMinutes(parseISO(this.currentDate), (hour * 60))
      const end = addMinutes(parseISO(this.currentDate), (hour * 60 + 59))
      const events = this.events.filter((event) => event.carId === carId)
      return events.filter((event) => {
        const eventStartHour = getHours(parseISO(event.start))
        const eventEndHour = getHours(addMinutes(parseISO(event.start), event.duration))

        if (!this.hours.includes(eventStartHour) && !this.hours.includes(eventEndHour)) return

        const eventEndTime = addMinutes(parseISO(event.start), event.duration)
        const isWithinStart = isWithinInterval(parseISO(event.start), { start, end })
        const isWithinEnd = isWithinInterval(eventEndTime, { start, end })

        if (this.hours.includes(eventStartHour)) return isWithinStart
        if (this.hours.includes(eventEndHour)) return isWithinEnd
      }) || []
    },
  },
}
</script>

<style lang="scss" scoped>
#layout {
  height: 100%;
  background: #fff;
  -webkit-user-select: none; /* Safari */
  -ms-user-select: none; /* IE 10 and IE 11 */
  user-select: none; /* Standard syntax */
}

#filters {
  height: 40px;
  color: #fff;
  background: dodgerblue;
}

#grid {
  width: 100%;
  height: 100vh;
  display: grid;
  grid-template-areas: "cars slider calendar";
}

.header {
  background: #ebebeb;
}
.cars {
  grid-area: cars;
  .car{
    height: 60px;
    border-bottom: 1px solid #ccc;
    box-sizing: border-box;
    display: flex;
    align-items: center;
  }
}

.slider {
  grid-area: slider;
  width: 5px;
  background: #ebebeb;
  cursor: col-resize;

  &:hover {
    box-shadow: 0 0 2px 0 rgba(0, 0, 0, 0.75);
  }
}
.calendar {
  grid-area: calendar;
  display: flex;
  flex-direction: column;
  .hours {
    display: flex;
    height: 100%;
    .hour {
      //border-bottom: 1px solid #ccc;
    }
  }
}



/* ********************  REUSABLE  ******************** */
.grid-layout {
  display: grid;
  grid-template-areas: "spacer slider time";
}

.scrollable-layout {
  display: flex;
  overflow-x: auto;
  //-ms-overflow-style: none;
  //scrollbar-width: none;
  //&::-webkit-scrollbar { display: none; }
  .layout-segment {
    flex: 1 0 70px;
    align-content: center;
    border-inline-start: 1px solid #ccc;
    border-bottom: 1px solid #ccc;
    &:first-child {border-left: none;}
  }
}


</style>
