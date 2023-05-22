<template>
  <div class="elevator-app">
    <div class="mine">
      <div class="elevator">
        <div v-for="floor in floors" :key="floor" class="elevator-floor">
          <button v-if="floor > 1" class="elevator-floor-order" @click="requestFloor(floor)"></button>
          <p v-if="floor > 1">{{ waiting[floors.indexOf(floor)] }}</p>
          <div class="elevator-cabin" v-if="currentFloor == floor">
            <p>{{ peopleOnElevator }}</p>
          </div>
        </div>
      </div>
      <MovingElevator class="box" :currentFloor="currentFloor" :targetFloor="targetFloor" :isMoving="isMoving"></MovingElevator>
    </div>
  </div>
</template>

<script>
import MovingElevator from './MovingElevator.vue';

export default {
  data() {
    return {
      currentFloor: 1,
      waiting: [0, 0, 0, 0, 0],
      floors: [5, 4, 3, 2, 1],
      peopleOnElevator: 0,
      maxFloor: 1,
      isMoving: false
    };
  },
  watch: {
    waiting: {
      handler(newArr) {
        if (newArr.every(item => item === 0)) {
          this.moveElevator(1);
        }
        else if (this.currentFloor < this.maxFloor) {
          this.moveElevator(this.maxFloor);
        }
        else {
          this.moveElevator(1);
        }
      },
      deep: true
    },
    currentFloor: {
      handler(floor) {
        if (floor === 1) {
          this.peopleOnElevator = 0;
          const unpickedFloor = this.getUnpickedFloor();
          if (unpickedFloor !== -1) {
            this.moveElevator(unpickedFloor);
          }
        }
      }
    }
  },
  methods: {
    requestFloor(floor) {
      this.waiting[this.floors.indexOf(floor)]++;
      this.maxFloor = Math.max(this.maxFloor, floor);
    },
    moveElevator(floor) {
    if (this.isMoving) return;
    this.isMoving = true;
    this.targetFloor = floor; 
    const delay = 1000;
    if (floor === this.currentFloor) {
      this.isMoving = false;
      return;
    }
    const moveStep = () => {
  const stepSize = 1; 
  const distance = Math.abs(floor - this.currentFloor);
  const direction = Math.sign(floor - this.currentFloor);

  if (distance <= stepSize) {
    this.currentFloor = floor;
    this.peopleOnElevator += this.waiting[this.floors.indexOf(this.currentFloor)];
    this.waiting[this.floors.indexOf(this.currentFloor)] = 0;
    clearInterval(interval);
    this.isMoving = false;
  } else {

    this.currentFloor += direction * stepSize;
    this.peopleOnElevator += this.waiting[this.floors.indexOf(this.currentFloor)];
    this.waiting[this.floors.indexOf(this.currentFloor)] = 0;

    if (direction === 1) {
      const unpickedFloor = this.getUnpickedFloorBelow(this.currentFloor);
      if (unpickedFloor !== -1) {
        clearInterval(interval);
        this.isMoving = false;
        this.moveElevator(unpickedFloor);
      }
    }
  }
};

    const interval = setInterval(moveStep, delay);
  },
    getUnpickedFloor() {
      for (let i = this.currentFloor + 1; i <= this.maxFloor; i++) {
        if (this.waiting[this.floors.indexOf(i)] > 0) {
          return i;
        }
      }
      return -1;
    },
    getUnpickedFloorBelow(floor) {
      for (let i = floor - 1; i >= 1; i--) {
        if (this.waiting[this.floors.indexOf(i)] > 0) {
          return i;
        }
      }
      return -1;
    }
  },
  components: { MovingElevator }
};


</script>

<style lang="scss">
.elevator-app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.mine{
  display: flex;
  flex-direction: row;
  align-items: end;
}

.elevator {
  width: 200px;
  background-color: #f0f0f0;
  padding: 20px;
  border-radius: 4px;
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: 50px;
}

.elevator-floor {
  border-color: black;
  border: 1px;
  width: 200px;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
}

.elevator-cabin {
  width: 20px;
  height: 20px;
  background-color: #0056b3;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>