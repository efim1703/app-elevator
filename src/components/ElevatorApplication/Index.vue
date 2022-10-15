<template>
  <div class="wrapper">
    <elevator-shaft 
      :settingsForPanelElevator="settingsForPanelElevator"
      v-for="index in settingsForApplication.numberOfElevatorShaft" 
      :key="index"
    />

    <div class="floors">
      <app-floor 
        v-for="index in settingsForApplication.numberOfFloors" 
        :key="index"
        :floorNumber="String(settingsForApplication.numberOfFloors+1-index)" 
        :buttonIsPressed="buttonIsPressed(settingsForApplication.numberOfFloors+1-index)"
        @elevator-call="elevatorCall($event)"
      />
    </div>
  </div>
</template>

<script>
import AppFloor from './AppFloor.vue'
import ElevatorShaft from './ElevatorShaft.vue'

export default {
  name: 'ElevatorApplication',
  components: {
    AppFloor,
    ElevatorShaft
  },
  props: {
    settingsForApplication: {
      type: Object,
      required: true
    }
  },
  mounted() {
    if (localStorage.getItem('floorNumberWithElevator')) {
      try {
        this.floorNumberWithElevator = JSON.parse(localStorage.getItem('floorNumberWithElevator'))
        document.querySelector('.elevator').style.transform = `translateY(calc(-${this.floorNumberWithElevator-1}00%))`
      } catch(e) {
        console.log(e);
      }
    }

    if (localStorage.getItem('callStack')) {
      try {
        this.callStack = JSON.parse(localStorage.getItem('callStack'))
        this.processingCallStack()
      } catch(e) {
        console.log(e);
      }
    }
  },
  data() {
    return {
      callStack: [],
      floorNumberWithElevator: 1,
      settingsForPanelElevator: {},
    }
  },
  watch: {
    callStack: function() {
      let callStack = JSON.stringify(this.callStack)
      localStorage.setItem("callStack", callStack)
    },
    floorNumberWithElevator: function() {
      let floorNumberWithElevator = JSON.stringify(this.floorNumberWithElevator)
      localStorage.setItem("floorNumberWithElevator", floorNumberWithElevator)
    },
  },
  methods: {
    buttonIsPressed(floorNumber) {
      return this.callStack.indexOf(String(floorNumber)) != -1 
    },
    elevatorCall(floorNumber) {
      if (floorNumber != this.floorNumberWithElevator) {
        if (this.callStack.indexOf(String(floorNumber)) === -1) {
          if (this.callStack.length == 0) {
            this.callStack.push(floorNumber)
            this.processingCallStack()
          } else {
            this.callStack.push(floorNumber)
          }
        } else {
          let button = document.querySelector(`.floor:nth-child(${this.settingsForApplication.numberOfFloors+1-floorNumber}) button`)

          button.classList.add('indication-animation')
          setTimeout(function() {
            button.classList.remove('indication-animation')
          }, 2000);
        }
      }
    },
    processingCallStack() {
     if (this.callStack.length != 0) {
        let firstFloorInCallStack = this.callStack[0]
        let floorDifference = firstFloorInCallStack - this.floorNumberWithElevator

        floorDifference > 0 ? this.goElevator(firstFloorInCallStack, floorDifference ,'top') : this.goElevator(firstFloorInCallStack, floorDifference ,'down')
        this.floorNumberWithElevator = firstFloorInCallStack
        setTimeout(this.relaxAnimation, Math.abs(floorDifference)*1000)
        setTimeout(() => {  
          this.callStack.shift();
          this.settingsForPanelElevator.show = false;
          this.processingCallStack();
        }, Math.abs(floorDifference)*1000 + 3000)
     }
    },
    goElevator(floorNumber, numberOfFloors, orientation) {
      this.settingsForPanelElevator = {
          show: true,
          floorNumber,
          orientation
      }

      let elevator = document.querySelector('.elevator')

      if (orientation === 'top') {
        elevator.style.transition = `${numberOfFloors}s`
        elevator.style.transform = `translateY(calc(-${this.floorNumberWithElevator-1}00% - ${Math.abs(numberOfFloors)}00%))`
      } else {
        elevator.style.transition = `${Math.abs(numberOfFloors)}s`
        elevator.style.transform = `translateY(calc(-${this.floorNumberWithElevator-1}00% + ${Math.abs(numberOfFloors)}00%))`
      }
    },
    relaxAnimation() {
      let elevator = document.querySelector('.elevator')
      
      elevator.classList.add('relax-animation')
      setTimeout(function() {
        elevator.classList.remove('relax-animation')
      }, 3000);
    }
  }
}

</script>

<style lang="scss" scoped>
.wrapper {
  display: flex;
  padding: 8px;
  width: 1044px;
  height: fit-content;
  border: 2px solid #212121;
  margin: 0 auto;

  & .floors {
    height: 100%;
    width: 100%;
  }
}
</style>