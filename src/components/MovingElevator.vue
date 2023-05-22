<template>
    <div class="smooth-elevator" :style="elevatorStyle"></div>
  </template>
  
  <script>
  export default {
    props: {
      currentFloor: {
        type: Number,
        required: true
      },
      targetFloor: {
        type: Number,
        required: true
      },
      duration: {
        type: Number,
        default: 1000
      }
    },
    computed: {
      elevatorStyle() {
        const distance = Math.abs(this.targetFloor - this.currentFloor);
        const animationDuration = `${this.duration * distance}ms`;
        const translateY = `${(this.currentFloor - 1) * (-100)}%`;
  
        return {
          transform: `translateY(${translateY})`,
          transition: `transform ${animationDuration} ease-in-out`
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
    transition: transform 1s ease-in-out;
  }
  </style>
  