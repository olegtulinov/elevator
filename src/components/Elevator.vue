<template>
  <div class="elevator"
    :style="{ transform: `translate(0, ${height}px)`, animation: `${animation}`, transition: `${time}ms` }">
    <template v-if="this.elevator.direction === 'up'">⭡</template>
    <template v-if="this.elevator.direction === 'down'">⭣</template>
    {{ this.elevator.floor }}
  </div>
</template>
<script>
export default {

  props: {
    elevator: {
      type: Object,
      required: true
    },
    time: {
      type: Number,
      required: true
    }
  },
  computed: {
    height() {
      return -(this.elevator.floor - 1) * 100
    },
    animation() {
      return this.elevator.direction === 'none' && !this.elevator.isFree ? "smoothBlink 1s infinite" : "none"
    }
  }
}
</script>
<style>
.elevator {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 90px;
  height: 100px;
  background-color: aqua;
  transition: 1s;
}
</style>
  