<template>
  <div id="layout">
    <div id="filters">Filters</div>
    <div id="grid" :style="gridTemplateColumn">
      <div class="cars">
        <div>name</div>
        <div>name</div>
        <div class="car" v-for="car in cars" :key="car.id">{{ car.model }}</div>
      </div>

      <div class="slider" @mousedown.left="mouseDownListener"></div>

      <div class="calendar scrollable-layout">
        <div>{{ currentDate }}</div>
        <div class="hours">
          <div v-for="hour in hours" :key="hour" class="hour layout-segment text-center">
            <div>{{ hour }}</div>
            <div style="height: 60px" class="line scrollable-layout" v-for="car in cars" :key="car.id+'i'">xx</div>
          </div>
        </div>
<!--        <div class="content">-->
<!--          <div class="line scrollable-layout" v-for="car in cars" :key="car.id+'i'">-->
<!--            <div class="cell layout-segment d-flex align-center justify-center" v-for="hour in hours" :key="hour+'k'"></div>-->
<!--          </div>-->
<!--        </div>-->
      </div>

    </div>

  </div>
</template>

<script>

// import vehiclesService from '@/services/vehiclesService'
import { format } from 'date-fns'

export default {
  name: 'VehiclesCalendar',
  components: {},
  data: () => ({
    date: new Date(),
    hours: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23],
    cars: [
      { id: 1, model: 'BMW', make: '328' }
    ],
    events: [
      { id: 1, start: '1:00', duration: 90 },
      { id: 2, start: '2:15', duration: 90 },
      { id: 3, start: '3:30', duration: 90 },
      { id: 4, start: '4:45', duration: 90 },
    ],
    spacerSize: 180,
    moving: false,
  }),
  mounted () {
    // this.fetch()
    //     .then()
    //     .catch((error) => console.log(error))
    //     .finally(() => {
    //       this.loadingPage = false
    //     })
  },
  computed: {
    gridTemplateColumn () {
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
    },
    mouseUpListener () {
      if (!this.moving) return
      this.moving = false
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
  display: grid;
  grid-template-areas: "cars slider calendar";
}

.header {
  background: #ebebeb;
}

.cars {
  grid-area: cars;
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
    height: calc(100vh - 130px);
  }
}

.cars {
  .car{
    height: 60px;
    border-bottom: 1px solid #ccc;
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
