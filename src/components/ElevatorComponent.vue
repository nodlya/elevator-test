<template>
  <div class="elevator-app">
    <div class="elevator">
      <div class="elevator-display">
        Current Floor: {{ currentFloor }}
      </div>
      <div class="elevator-buttons">
        <button v-for="floor in floors" :key="floor" @click="requestFloor(floor)">
          {{ floor }}
          <button v-if="currentFloor === floor" class="elevator-box"></button>
        </button>

      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      currentFloor: 1,
      floors: [5, 4, 3, 2, 1],
    };
  },
  methods: {
    requestFloor(floor) {
      let max = Math.max(this.currentFloor, floor);
      let min = Math.min(floor, this.currentFloor);

      let temp = this.currentFloor - floor;

      const delay = 1000;

      if (temp < 0) {
        let i = min;
        const interval = setInterval(() => {
          this.currentFloor = i;
          i++;
          if (i > max) {
            clearInterval(interval);
          }
        }, delay);
      } else {
        let i = max;
        const interval = setInterval(() => {
          this.currentFloor = i;
          i--;
          if (i < min) {
            clearInterval(interval);
          }
        }, delay);
      }
    },
  },
};
</script>

<style>
.elevator-app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.elevator {
  width: 200px;
  background-color: #f0f0f0;
  padding: 20px;
  border-radius: 4px;
  text-align: center;
}

.elevator-display {
  font-size: 24px;
  margin-bottom: 20px;
}

.elevator-buttons button {
  display: block;
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.elevator-buttons button:hover {
  background-color: #0056b3;
}

.elevator-box {
  width: 20px;
  border-color: #0056b3;
  background-color: white;

}
</style>
