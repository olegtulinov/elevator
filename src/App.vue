<template>
  <div class="wrap">
    <div class="shaft">
      <div class="elevator">
      </div>
    </div>
    <div class="buttons">
      <button class="button" v-for=" floor  in  floors " :key="floor">
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
      firstTimeOut: null,
      secondTimeOut: null,
      elevators: [
        { id: 1, floor: 1, isFree: true, direction: "none", inProgress: 'none' },
      ],
    }
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
  /* cursor: pointer; */
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