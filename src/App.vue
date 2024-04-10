<template>
  <div class="wrap">
    <div class="shaft">
      <div class="elevator">
        <template v-if="this.elevators.direction === 'up'">⭡</template>
        <template v-if="this.elevators.direction === 'down'">⭣</template>
        {{ this.elevators[0].floor }}
      </div>
    </div>
    <div class="buttons">
      <button @click="elevatorCall(floor)" class="button" v-for=" floor  in  floors " :key="floor">
        {{ floor }}
      </button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      floors: 5,
      floor: 1,
      callList: [],
      time: 0,
      elevators: [
        { id: 1, floor: 1, isFree: true, direction: "none", inProgress: 'none' },
      ],
    }
  },
  methods: {
    setDirection(floor, id) {
      if (this.elevators[id].floor > floor) {
        this.elevators[id].direction = 'down'
      } else {
        this.elevators[id].direction = 'up'
      }
    },

    elevatorCall(floor) {
      if (this.elevators.some((item => item.floor === floor))) {
        return false;
      }

      if (this.callList.includes(floor)) {
        return false;
      }

      this.callList.push(floor)
    },

  },
}
</script>

<style>
.wrap {
  display: flex;
}

.buttons {
  display: flex;
  flex-direction: column-reverse;
  padding: 25px 10px;
}

.button {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 35px;
  width: 35px;
  margin: auto 0;
  padding: 18px;
  border-radius: 50%;
  border: 1px solid #111;
}

.elevator {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 90px;
  height: 100px;
  background-color: aqua;
  transition: 1s;
}

.shaft {
  display: flex;
  align-items: end;
  width: 90px;
  height: 500px;
  border: 1px solid #111;
  margin: 10px;
}
</style>