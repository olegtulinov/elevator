<template>
    <div class="wrap">
        <div v-for="elevator in elevators" :key="elevator.id" class="shaft" :style="{ height: `${heightShaft}px` }">
            <Elevator :elevator="elevator" :time="time" />
        </div>

        <div class="buttons">
            <button v-for="floor in floors " :key="floor"
                :class="['button', { 'wait': this.elevators.some((item) => item.inProgress === floor || this.callList.includes(floor)) },]"
                @click="elevatorCall(floor)">
                {{ floor }}
            </button>
        </div>
    </div>
</template>
  
<script>
import Elevator from './Elevator.vue';

export default {
    name: 'Hall',

    components: {
        Elevator
    },

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

    mounted() {
        const elevatorInfoJson = localStorage.getItem('elevatorInfo');

        if (elevatorInfoJson) {
            const { callList, elevators } = JSON.parse(elevatorInfoJson);

            this.callList = callList;
            this.elevators = elevators;
        }
        this.resetElevator()
    },

    computed: {
        heightShaft() {
            return this.floors * 100
        },

        delay() {
            return this.time + 3000
        }
    },

    watch: {
        callList: {
            handler() {
                this.startElevator();
            },
            deep: true
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

        selectElevator(floor) {
            return this.elevators
                .filter(item => item.isFree)
                .map(item => ({ id: item.id, floor: Math.abs(item.floor - floor) }))
                .sort((a, b) => a.floor - b.floor)[0].id
        },

        calcTime(id) {
            this.time = this.floor > this.elevators[id].floor ? (this.floor - this.elevators[id].floor) * 1000 : (this.elevators[id].floor - this.floor) * 1000
        },
        resetElevator() {
            this.elevators = this.elevators.map((item) => { return { id: item.id, floor: item.floor, isFree: true, direction: 'none', inProgress: 'none' } })
        },

        startElevator() {
            if (this.callList.length && this.elevators.some(item => item.isFree === true)) {
                this.floor = this.callList.shift()
                const id = (this.selectElevator(this.floor) - 1)
                this.elevators[id].isFree = false
                this.calcTime(id);
                this.elevators[id].inProgress = this.floor
                this.setDirection(this.floor, id)
                this.elevators[id].floor = this.floor
                this.firstTimeOut = setTimeout(() => {
                    this.elevators[id].inProgress = 'none'
                    this.elevators[id].direction = 'none'
                }, this.time)

                this.secondTimeOut = setTimeout(() => {
                    this.elevators[id].isFree = true
                    localStorage.setItem('elevatorInfo', JSON.stringify({
                        elevators: this.elevators,
                        callList: this.callList,
                    }));
                    this.startElevator()
                }, this.delay)

            }
        },
    },

    unmounted() {
        clearTimeout(this.firstTimeOut)
        clearTimeout(this.secondTimeOut)
    }
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
    cursor: pointer;
}

.wait {
    background-color: tomato;
}

.inProgress {
    background-color: rgb(253, 132, 40);
}

.shaft {
    display: flex;
    align-items: end;
    margin: 10px;
    width: 90px;
    height: 500px;
    border: 1px solid #111;
}

@keyframes smoothBlink {

    0%,
    100% {
        opacity: 1;
    }

    50% {
        opacity: 0;
    }
}
</style>
  