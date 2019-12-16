<template>
  <div id="app">
    <div class="grid">
      <div
        class="hour"
        v-for="hour in hours"
        :key="hour"
        :style="{ gridRowStart: 1 + hour - startHour }"
      >
        {{ hour }}
      </div>
      <div
        class="delivery-slot"
        v-for="slot in augmentedSlots"
        :key="slot.start + '-' + slot.end"
        :style="{
          gridRowStart: 1 + slot.start - startHour,
          gridRowEnd: 1 + slot.end - startHour,
          gridColumn: `span ${slot.width}`
        }"
      >
        {{ slot.start }}:00 - {{ slot.end }}:00 <br />
        Parallel: {{ slot.parallelSlots }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  data() {
    return {
      startHour: 8,
      endHour: 22,
      deliverySlots: [
        { start: 8, end: 10 },
        { start: 8, end: 18 },
        { start: 10, end: 12 },
        { start: 10, end: 16 },
        { start: 12, end: 24 },
        { start: 16, end: 18 },
      ]
    };
  },
  computed: {
    hours() {
      var list = [];
      for (var i = this.startHour; i <= this.endHour; i++) {
        list.push(i);
      }
      return list;
    },
    augmentedSlots() {
      const tempArray = this.deliverySlots.map(slot => {
        return {
          ...slot,
          parallelSlots: this.getParallelSlotsBetween(slot.start, slot.end)
        };
      });
      const maxParallelSlots = Math.max(...tempArray.map(slot=>slot.parallelSlots)) || 1
      return tempArray.map(slot=>{return{
        ...slot,
        width: Math.round(maxParallelSlots/ slot.parallelSlots)
      }})
    },
  },
  methods: {
    getParallelSlotsBetween(start, end) {
      var list = [];
      for (var i = start; i < end; i++) {
        list.push(this.getParallelSlots(i));
      }    
      return Math.max(...list);
    },
    getParallelSlots(hour) {
      return this.deliverySlots.filter(
        slot => slot.end > hour && slot.start <= hour
      ).length;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.grid {
  display: grid;
  grid-auto-flow: dense;
  background: #eee;
  grid-template-columns: 40px auto;
}
.hour {
  grid-column-start: 1;
  align-content: center;
  justify-content: center;
  height: 40px;
  width: 40px;
  background: #ddd;
  display: grid;
  align-content: center;
}
.delivery-slot {
  background: #fff;
  border: 2px solid #3d729e;
  color: #3d729e;
  border-radius: 8px;
  margin: 4px;
  display: grid;
  align-content: center;
}
</style>
