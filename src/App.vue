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
  gap: 75px
}

.button {
  padding: 5px 10px;
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
}
</style>