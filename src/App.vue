<template>
  <!--<img alt="Vue logo" src="./assets/logo.png">-->

  <SchematicElevator/>
  <!-- <ElevatorCabin
      :currentFloor="currentFloor"
      :isMoving="isMoving"
      :isAccelerating="isAccelerating"
      :elevatorPosition="elevatorPosition"
    /> -->
</template>

<script>
//import ElevatorComponent from './components/ElevatorComponent.vue'
import SchematicElevator from './components/SchematicElevator.vue';
//import ElevatorCabin from './components/ElevatorCabin.vue';


export default {
  name: 'App',
  components: {
    SchematicElevator,
    //ElevatorCabin
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
