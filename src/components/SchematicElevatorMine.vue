<template>
  <div class="elevator-app">
    <div class="mine">
      <div class="elevator">
        <div v-for="floor in floors" :key="floor" class="elevator-floor">
          <p>Этаж {{ floor }}</p>
          <p v-if="floor === 1">Доставлено {{ deliveredPeople }}</p>
          <button v-if="floor > 1" class="elevator-floor-order" @click="requestFloor(floor)">
            <p v-if="floor > 1">{{ waiting[floors.indexOf(floor)] }}</p>
          </button>
        </div>
      </div>
      <MovingElevator class="box" :currentFloor="currentFloor" :peopleCount="peopleOnElevator">
      </MovingElevator>
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
      isMoving: false,
      deliveredPeople: 0
    };
  },

  watch: {
    waiting: {
      handler(newArr) {
        if (newArr.every(item => item === 0)) {
          const nextFloor = this.getNextFloor(this.currentFloor, false);
          if (nextFloor !== -1) {
            this.moveElevator(nextFloor);
          }
        } else {
          const nextFloor = this.getNextFloor(this.currentFloor, true);
          if (nextFloor !== -1) {
            this.moveElevator(nextFloor);
          }
        }
      },
      deep: true
    },

    currentFloor: {
      handler(floor) {
        if (floor === 1) {
          this.deliveredPeople += this.peopleOnElevator;
          this.peopleOnElevator = 0;
          const nextFloor = this.getNextFloor(this.currentFloor, true);

          if (nextFloor !== -1) {
            this.moveElevator(nextFloor);

          } else {
            const nextFloorBelow = this.getNextFloor(this.currentFloor, false);

            if (nextFloorBelow !== -1) {
              this.moveElevator(nextFloorBelow);
            }
          }
        }
        else if (this.isMoving && floor < this.currentFloor) {
          return;
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

      const interval = setInterval(() => {
        const stepSize = 1;
        const distance = Math.abs(floor - this.currentFloor);
        const direction = Math.sign(floor - this.currentFloor);

        if (distance <= stepSize) {
          this.currentFloor = floor;
          this.peopleOnElevator += this.waiting[this.floors.indexOf(this.currentFloor)];
          this.waiting[this.floors.indexOf(this.currentFloor)] = 0;
          clearInterval(interval);
          this.isMoving = false;

          if (floor === this.maxFloor && floor !== 1) {
            const nextFloor = 1;
            this.moveElevator(nextFloor);
          } else if (this.currentFloor === 1 && this.maxFloor > 1) {
            const nextFloorAbove = this.getNextFloor(this.currentFloor, true);
            if (nextFloorAbove !== -1) {
              this.moveElevator(nextFloorAbove);
            }
          } else {
            const nextFloor = this.getNextFloor(this.currentFloor, true); 
            if (nextFloor !== -1) {
              this.moveElevator(nextFloor);
            }
          }
        } else {
          this.currentFloor += direction * stepSize;
          this.peopleOnElevator += this.waiting[this.floors.indexOf(this.currentFloor)];
          this.waiting[this.floors.indexOf(this.currentFloor)] = 0;
        }
      }, delay);
    },


    getNextFloor(floor, above) {
      const startIndex = above ? floor + 1 : floor - 1;
      const endIndex = above ? this.maxFloor : 1;
      const step = above ? 1 : -1;

      for (let i = startIndex; above ? i <= endIndex : i >= endIndex; i += step) {
        if (this.waiting[this.floors.indexOf(i)] > 0) {
          return i;
        }
      }

      return above ? -1 : 1;
    },
  },

  components: { MovingElevator }
};
</script>

<style lang="scss">
.elevator-app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}

.mine {
  display: flex;
  flex-direction: row;
  align-items: end;
  background-color: #f0f0f0;
  height: auto;
}

.elevator {
  width: 200px;
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
  gap: 10px;
}

.elevator-floor-order {
  position: relative;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  overflow: hidden;

  :hover::before {
    content: "";
    position: absolute;
    top: -2px;
    left: -2px;
    width: calc(100% + 4px);
    height: calc(100% + 4px);
    border-radius: 50%;
    background-color: #7d7d7d;
  }
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