<template>
  <div class="smooth-elevator" :style="elevatorStyle">
    <p>Этаж {{ currentFloor }}</p>
    <p>{{ peopleCount }}</p>
  </div>
</template>

<script>
export default {
  props: {

    currentFloor: {
      type: Number,
      required: true
    },

    durationPerFloor: {
      type: Number,
      default: 1000
    },

    peopleCount: {
      type: Number,
      required: true
    }
  },

  computed: {
    elevatorStyle() {
      const translateY = `${(this.currentFloor - 1) * (-100)}%`;

      return {
        transform: `translateY(${translateY})`,
        transition: `transform ${this.durationPerFloor}ms ease-in-out`
      };
    }
  }
};
</script>

<style scoped>
.smooth-elevator {
  width: 100px;
  height: 100px;
  background-color: rebeccapurple;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: transform 1s ease-in-out;
}
</style>
